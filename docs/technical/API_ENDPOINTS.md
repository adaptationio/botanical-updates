# API Endpoints Documentation

## Base URLs

- **Production**: `https://api.botaniqal.com.au`
- **Staging**: `https://staging-api.botaniqal.com.au`
- **Local**: `http://localhost:3000`

## Authentication

All API requests require authentication using JWT tokens.

```
Authorization: Bearer <token>
```

## Booking Endpoints

### Create Free Consultation
```
POST /api/bookings/consultation
```

**Request Body:**
```json
{
  "patientName": "string",
  "email": "string",
  "phone": "string",
  "preferredTime": "ISO-8601 datetime",
  "timezone": "string"
}
```

**Response:**
```json
{
  "bookingId": "string",
  "consultantName": "string",
  "scheduledTime": "ISO-8601 datetime",
  "meetingLink": "string"
}
```

### Create Practitioner Booking
```
POST /api/bookings/practitioner
```

**Request Body:**
```json
{
  "patientId": "string",
  "practitionerId": "string",
  "serviceType": "alternative-medicine|gaps|weight-loss",
  "appointmentType": "initial|follow-up",
  "scheduledTime": "ISO-8601 datetime",
  "paymentMethod": "phone|online",
  "amountPaid": "number"
}
```

### Get Available Slots
```
GET /api/availability/{practitionerId}
```

**Query Parameters:**
- `startDate`: ISO-8601 date
- `endDate`: ISO-8601 date
- `serviceType`: Service type filter

**Response:**
```json
{
  "practitionerId": "string",
  "availableSlots": [
    {
      "datetime": "ISO-8601 datetime",
      "duration": "number (minutes)",
      "serviceTypes": ["string"]
    }
  ]
}
```

## Patient Endpoints

### Get Patient Details
```
GET /api/patients/{patientId}
```

### Update Patient Information
```
PUT /api/patients/{patientId}
```

### Get Patient Bookings
```
GET /api/patients/{patientId}/bookings
```

## Form Endpoints

### Submit Mini Intake Form
```
POST /api/forms/mini-intake
```

**Request Body:**
```json
{
  "bookingId": "string",
  "patientData": {
    "name": "string",
    "dateOfBirth": "date",
    "contactDetails": "object"
  },
  "healthSummary": "string",
  "consentSignature": "string",
  "consentDate": "ISO-8601 datetime"
}
```

### Submit Full Intake Form
```
POST /api/forms/full-intake
```

### Submit Dynamic Triage Form
```
POST /api/forms/triage
```

## Webhook Endpoints

### Calendly Webhook
```
POST /webhooks/calendly
```

**Headers:**
- `X-Calendly-Signature`: HMAC signature for verification

### Office 365 Calendar Webhook
```
POST /webhooks/office365
```

### JotForm Webhook
```
POST /webhooks/jotform
```

## Admin Endpoints

### Sync Calendars
```
POST /api/admin/sync-calendars
```

### Get Booking Statistics
```
GET /api/admin/statistics
```

**Query Parameters:**
- `startDate`: ISO-8601 date
- `endDate`: ISO-8601 date
- `groupBy`: day|week|month

### Manual Booking Entry
```
POST /api/admin/manual-booking
```

## Error Responses

All endpoints return standard error responses:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

**Common Error Codes:**
- `AUTH_REQUIRED`: Authentication token missing
- `AUTH_INVALID`: Invalid authentication token
- `NOT_FOUND`: Resource not found
- `VALIDATION_ERROR`: Request validation failed
- `CONFLICT`: Resource conflict (e.g., double booking)
- `INTERNAL_ERROR`: Server error

## Rate Limiting

- **Default**: 100 requests per minute per IP
- **Authenticated**: 1000 requests per minute per user
- **Webhooks**: No rate limit

**Rate Limit Headers:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1625234567
```

## Pagination

List endpoints support pagination:

```
GET /api/patients/{patientId}/bookings?page=1&limit=20
```

**Response Headers:**
```
X-Total-Count: 156
X-Page-Count: 8
```

---
*Note: This is a template. Update with actual implementation details.*