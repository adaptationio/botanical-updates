# Technical Specifications for Booking System Update

## System Architecture Changes

### Current Architecture
```
[Webflow] → [Calendly Widget] → [Single Calendar] → [Webhook] → [Lambda] → [MediRecords]
                                        ↓
                                 [JotForm Intake]
```

### New Architecture
```
[Webflow] → [Triage Calendar] → [Webhook] → [Lambda] → [Triage Session]
                                                ↓
                                    [Phone Booking System]
                                                ↓
                        [6 Practitioner Calendars] → [MediRecords]
```

## Required Infrastructure

### AWS Lambda Functions

#### 1. Triage Webhook Handler
```javascript
// Lambda: botaniqal-triage-webhook
exports.handler = async (event) => {
  const { body, headers } = event;
  
  // Verify Calendly signature
  if (!verifyCalendlySignature(headers, body)) {
    return { statusCode: 401 };
  }
  
  const calendlyEvent = JSON.parse(body);
  
  if (calendlyEvent.event === 'invitee.created') {
    // Create triage session
    const session = await db.triageSessions.create({
      patientEmail: calendlyEvent.payload.email,
      patientName: calendlyEvent.payload.name,
      patientPhone: calendlyEvent.payload.questions_and_answers?.[0]?.answer,
      scheduledTime: calendlyEvent.payload.event.start_time,
      consultantId: mapCalendlyToConsultant(calendlyEvent.payload.event.calendar_id),
      status: 'scheduled'
    });
    
    // Send preparation email
    await ses.sendEmail({
      to: calendlyEvent.payload.email,
      template: 'triage-preparation',
      data: {
        consultantName: session.consultantName,
        scheduledTime: session.scheduledTime,
        zoomLink: calendlyEvent.payload.event.location.join_url
      }
    });
  }
  
  return { statusCode: 200 };
};
```

#### 2. Practitioner Booking Handler
```javascript
// Lambda: botaniqal-practitioner-webhook
exports.handler = async (event) => {
  const calendlyEvent = JSON.parse(event.body);
  const calendarId = calendlyEvent.payload.event.calendar_id;
  
  // Map calendar to practitioner
  const practitioner = await getPractitionerByCalendarId(calendarId);
  
  if (!practitioner) {
    console.error('Unknown calendar ID:', calendarId);
    return { statusCode: 400 };
  }
  
  // This should only receive bookings made by consultants
  const booking = await db.bookings.create({
    practitionerId: practitioner.id,
    patientEmail: calendlyEvent.payload.email,
    scheduledStart: calendlyEvent.payload.event.start_time,
    scheduledEnd: calendlyEvent.payload.event.end_time,
    serviceType: extractServiceType(calendlyEvent),
    appointmentType: extractAppointmentType(calendlyEvent),
    bookingSource: 'phone', // All practitioner bookings come via phone
    calendlyEventId: calendlyEvent.payload.event.uuid,
    status: 'confirmed'
  });
  
  // Sync to MediRecords
  await syncToMediRecords(booking, practitioner);
  
  return { statusCode: 200 };
};
```

#### 3. Phone Booking API
```javascript
// Lambda: botaniqal-phone-booking
exports.handler = async (event) => {
  const { method, path, body, headers } = event;
  
  // Authenticate consultant
  const consultant = await authenticateConsultant(headers.authorization);
  if (!consultant) {
    return { statusCode: 401 };
  }
  
  if (method === 'POST' && path === '/phone-booking') {
    const {
      triageSessionId,
      practitionerId,
      scheduledTime,
      paymentCollected,
      paymentMethod,
      amountCollected
    } = JSON.parse(body);
    
    // Validate triage session
    const triageSession = await db.triageSessions.findById(triageSessionId);
    if (!triageSession || triageSession.consultantId !== consultant.id) {
      return { statusCode: 403 };
    }
    
    // Check practitioner availability
    const available = await checkAvailability(practitionerId, scheduledTime);
    if (!available) {
      return { 
        statusCode: 409,
        body: JSON.stringify({ error: 'Time slot no longer available' })
      };
    }
    
    // Create booking via Calendly API
    const calendlyBooking = await createCalendlyBooking({
      practitionerId,
      patientEmail: triageSession.patientEmail,
      scheduledTime
    });
    
    // Record in database
    const booking = await db.bookings.create({
      triageSessionId,
      practitionerId,
      patientId: triageSession.patientId,
      scheduledStart: scheduledTime,
      bookingSource: 'phone',
      createdBy: consultant.id,
      paymentStatus: paymentCollected ? 'collected' : 'pending',
      paymentMethod,
      amountPaid: amountCollected,
      calendlyEventId: calendlyBooking.uuid
    });
    
    // Update triage session
    await db.triageSessions.update(triageSessionId, {
      outcome: 'booked',
      bookingId: booking.id
    });
    
    return {
      statusCode: 200,
      body: JSON.stringify({ bookingId: booking.id })
    };
  }
};
```

### Database Schema Updates

```sql
-- New tables needed

CREATE TABLE practitioners (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL CHECK (type IN ('doctor', 'nurse', 'consultant', 'coach')),
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    calendly_calendar_id VARCHAR(255) UNIQUE,
    medirecords_practitioner_id VARCHAR(255),
    location VARCHAR(50) CHECK (location IN ('telehealth', 'melbourne', 'both')),
    specializations TEXT[],
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE triage_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    patient_email VARCHAR(255) NOT NULL,
    patient_name VARCHAR(255) NOT NULL,
    patient_phone VARCHAR(20),
    consultant_id UUID REFERENCES practitioners(id),
    scheduled_time TIMESTAMP NOT NULL,
    actual_start_time TIMESTAMP,
    actual_end_time TIMESTAMP,
    primary_concern TEXT,
    triage_notes TEXT,
    recommended_practitioners UUID[],
    recommended_service VARCHAR(50),
    outcome VARCHAR(50) CHECK (outcome IN ('booked', 'referred', 'rejected', 'no_show')),
    booking_id UUID,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Modifications to existing tables

ALTER TABLE bookings
ADD COLUMN practitioner_id UUID REFERENCES practitioners(id),
ADD COLUMN triage_session_id UUID REFERENCES triage_sessions(id),
ADD COLUMN service_type VARCHAR(50),
ADD COLUMN booking_source VARCHAR(50) DEFAULT 'online',
ADD COLUMN created_by_user_id UUID,
ADD COLUMN payment_collected_by VARCHAR(50);

-- Remove hard-coded doctor_id default
ALTER TABLE bookings 
ALTER COLUMN doctor_id DROP DEFAULT;
```

### Calendly API Integration

```javascript
// calendly-service.js

class CalendlyService {
  constructor() {
    this.apiKey = process.env.CALENDLY_API_KEY;
    this.baseUrl = 'https://api.calendly.com';
  }
  
  // Get all organization calendars
  async getOrganizationCalendars() {
    const response = await fetch(`${this.baseUrl}/event_types`, {
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      }
    });
    
    return response.json();
  }
  
  // Create booking programmatically (for phone bookings)
  async createBooking(practitionerCalendarId, patientData, scheduledTime) {
    // Note: Calendly doesn't support creating bookings via API
    // Alternative approach: Create in our system and sync
    
    // Option 1: Use scheduling links with pre-filled data
    const schedulingLink = await this.createOneTimeSchedulingLink({
      eventTypeId: practitionerCalendarId,
      name: patientData.name,
      email: patientData.email,
      scheduledTime: scheduledTime
    });
    
    // Option 2: Direct calendar integration via Office 365
    // Implement if Calendly API limitations are problematic
  }
  
  // Monitor practitioner availability
  async getPractitionerAvailability(practitionerId, dateRange) {
    const practitioner = await db.practitioners.findById(practitionerId);
    
    const response = await fetch(
      `${this.baseUrl}/event_types/${practitioner.calendly_event_type_id}/available_times`,
      {
        headers: {
          'Authorization': `Bearer ${this.apiKey}`
        },
        params: {
          start_time: dateRange.start,
          end_time: dateRange.end
        }
      }
    );
    
    return response.json();
  }
}
```

### Environment Variables

```bash
# .env.production

# Existing
CALENDLY_API_KEY=xxx
MEDIRECORDS_API_KEY=xxx
DATABASE_URL=xxx

# New Required
CALENDLY_TRIAGE_CALENDAR_ID=xxx
CALENDLY_WEBHOOK_SIGNING_KEY=xxx

# Practitioner Calendar IDs
CALENDLY_DR_DIA_CALENDAR_ID=xxx
CALENDLY_DR_SHIVANI_CALENDAR_ID=xxx
CALENDLY_NURSE_CALENDAR_ID=xxx
CALENDLY_GAPS_COACH_CALENDAR_ID=xxx
CALENDLY_CONSULTANT_CALENDAR_ID=xxx

# Phone Booking System
PHONE_BOOKING_API_KEY=xxx
CONSULTANT_JWT_SECRET=xxx
```

### API Specifications

#### 1. Free Consultation Booking
```
POST /api/consultations/book

Request:
{
  "patientName": "John Doe",
  "email": "john@example.com",
  "phone": "+61412345678",
  "preferredTimes": ["2024-02-01T10:00:00Z", "2024-02-01T14:00:00Z"],
  "primaryConcern": "Digestive issues",
  "timezone": "Australia/Sydney"
}

Response:
{
  "consultationId": "uuid",
  "scheduledTime": "2024-02-01T10:00:00Z",
  "consultantName": "Sarah Consultant",
  "zoomLink": "https://zoom.us/...",
  "preparationEmailSent": true
}
```

#### 2. Practitioner Availability
```
GET /api/practitioners/availability?service=alternative_medicine&date=2024-02-01

Response:
{
  "practitioners": [
    {
      "id": "uuid",
      "name": "Dr. Dia",
      "type": "doctor",
      "location": "telehealth",
      "nextAvailable": "2024-02-01T14:00:00Z",
      "availableSlots": [
        {
          "time": "2024-02-01T14:00:00Z",
          "duration": 15
        },
        {
          "time": "2024-02-01T15:00:00Z",
          "duration": 15
        }
      ]
    },
    {
      "id": "uuid",
      "name": "Dr. Shivani",
      "type": "doctor", 
      "location": "melbourne",
      "nextAvailable": "2024-02-02T10:00:00Z",
      "availableSlots": [...]
    }
  ]
}
```

#### 3. Phone Booking Creation
```
POST /api/bookings/phone

Headers:
Authorization: Bearer {consultant_jwt_token}

Request:
{
  "triageSessionId": "uuid",
  "practitionerId": "uuid",
  "serviceType": "alternative_medicine",
  "appointmentType": "initial",
  "scheduledTime": "2024-02-01T14:00:00Z",
  "paymentCollected": true,
  "paymentMethod": "credit_card_phone",
  "amountCollected": 119.00,
  "notes": "Patient prefers afternoon appointments"
}

Response:
{
  "bookingId": "uuid",
  "confirmationNumber": "BOT-2024-0201",
  "calendlyEventCreated": true,
  "medirecordsSynced": true,
  "patientNotified": true
}
```

### Security Considerations

#### Consultant Authentication
```javascript
// JWT token for consultants with limited scope
const generateConsultantToken = (consultant) => {
  return jwt.sign(
    {
      sub: consultant.id,
      role: 'consultant',
      permissions: [
        'triage:read',
        'triage:update',
        'booking:create',
        'practitioner:availability'
      ],
      exp: Math.floor(Date.now() / 1000) + (8 * 60 * 60) // 8 hour sessions
    },
    process.env.CONSULTANT_JWT_SECRET
  );
};
```

#### Webhook Verification
```javascript
const verifyCalendlyWebhook = (headers, body) => {
  const signature = headers['calendly-webhook-signature'];
  const timestamp = headers['calendly-webhook-timestamp'];
  
  const payload = `${timestamp}.${body}`;
  const expectedSignature = crypto
    .createHmac('sha256', process.env.CALENDLY_WEBHOOK_SIGNING_KEY)
    .update(payload)
    .digest('base64');
    
  return signature === expectedSignature;
};
```

### Monitoring and Alerts

```javascript
// CloudWatch Metrics to Track
const METRICS = {
  // Booking funnel
  'triage_sessions_created': 'Count',
  'triage_to_booking_conversion': 'Percentage',
  'phone_bookings_created': 'Count',
  
  // System health
  'calendar_sync_failures': 'Count',
  'webhook_processing_time': 'Milliseconds',
  'available_slots_per_practitioner': 'Count',
  
  // Business metrics
  'revenue_per_booking_method': 'Currency',
  'practitioner_utilization': 'Percentage',
  'no_show_rate_by_type': 'Percentage'
};

// Alert Thresholds
const ALERTS = {
  'calendar_sync_failure': {
    threshold: 3,
    period: '5 minutes',
    action: 'page_oncall'
  },
  'no_available_practitioners': {
    threshold: 1,
    period: '1 hour',
    action: 'notify_operations'
  },
  'high_triage_abandonment': {
    threshold: 0.3, // 30%
    period: '1 day',
    action: 'notify_management'
  }
};
```

## Testing Requirements

### Integration Tests

```javascript
describe('Multi-Practitioner Booking Flow', () => {
  test('Free consultation creates triage session', async () => {
    const mockCalendlyEvent = createMockTriageBookingEvent();
    
    const response = await lambda.invoke({
      functionName: 'botaniqal-triage-webhook',
      payload: mockCalendlyEvent
    });
    
    expect(response.statusCode).toBe(200);
    
    const session = await db.triageSessions.findOne({
      patientEmail: mockCalendlyEvent.payload.email
    });
    
    expect(session).toBeDefined();
    expect(session.status).toBe('scheduled');
  });
  
  test('Phone booking creates all required records', async () => {
    const triageSession = await createTestTriageSession();
    const consultant = await createTestConsultant();
    
    const booking = await api.post('/bookings/phone', {
      triageSessionId: triageSession.id,
      practitionerId: testPractitioner.id,
      scheduledTime: '2024-02-01T14:00:00Z',
      paymentCollected: true
    }, {
      headers: {
        'Authorization': `Bearer ${consultant.token}`
      }
    });
    
    expect(booking.status).toBe(200);
    expect(booking.data.calendlyEventCreated).toBe(true);
    expect(booking.data.medirecordsSynced).toBe(true);
  });
});
```

### Load Testing

```yaml
# k6 load test script
scenarios:
  triage_booking:
    executor: 'constant-arrival-rate'
    rate: 10  # 10 bookings per second
    duration: '5m'
    preAllocatedVUs: 50
    
  practitioner_availability:
    executor: 'constant-arrival-rate'
    rate: 50  # 50 requests per second
    duration: '5m'
    preAllocatedVUs: 100
```

---

*This technical specification provides concrete implementation details for developers.*