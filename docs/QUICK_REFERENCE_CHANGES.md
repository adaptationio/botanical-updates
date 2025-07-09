# Quick Reference: Key Changes

## 🔄 Core Transformation

**From:** Single doctor, pay-first booking  
**To:** Multi-practitioner system with free triage consultation

## 📋 Change Summary

### User Experience Changes

| Before | After |
|--------|-------|
| Pay $89 AUD upfront (initial) | Free 20-min consultation first |
| Book with Dr. Dia only | 6 practitioners to choose from |
| Fill intake form after booking | Assessment during consultation |
| Online payment only | Phone payment option available |

### Technical Changes

| Component | Before | After |
|-----------|--------|-------|
| **Calendars** | 1 (Dr. Dia) | 7 (Triage + 6 practitioners) |
| **Booking Flow** | Direct → Pay → Form | Free Consult → Triage → Phone Book |
| **Payment** | Calendly integration | Phone collection by consultant |
| **Database** | Single doctor field | Multi-practitioner support |

## 🏗️ What Needs Building

### 1. New Database Tables
```sql
- practitioners (6 records needed)
- triage_sessions 
- Update bookings table (add practitioner_id, triage_session_id)
```

### 2. New Calendly Calendars
```
✅ Triage Calendar (FREE, 20 min)
✅ Dr. Dia - Telehealth
✅ Dr. Shivani - Telehealth & Melbourne 
✅ Nurse Practitioner - Telehealth
✅ Ramona (GAPS Coach)
✅ Consultant (for internal use)
```

### 3. New Lambda Functions
```
- botaniqal-triage-webhook
- botaniqal-practitioner-webhook  
- botaniqal-phone-booking
```

### 4. New UI Components
```
- Free consultation booking widget
- Practitioner selection cards
- Phone booking interface (admin)
- Multi-calendar availability view
```

## 🔌 Integration Points

### Calendly Webhooks
```
OLD: Single webhook → Single handler
NEW: Multiple webhooks → Routed handlers
```

### MediRecords Sync
```
OLD: Dr. Dia appointments only
NEW: All practitioner appointments with IDs
```

### Payment Processing
```
OLD: Calendly Stripe integration
NEW: Manual phone payment recording
```

## 💰 Pricing Structure

### Current vs New Pricing
| Service | Current | New | Change |
|---------|---------|-----|--------|
| **Initial Consultation** | $89 AUD (15 min) | $119 AUD (15 min) | +$30 AUD |
| **Follow-up** | $69 AUD (10 min) | $79 AUD (10 min) | +$10 AUD |
| **Triage Assessment** | N/A | FREE (20 min) | NEW |

### New Services & Pricing
| Service | Initial | Follow-up | Location |
|---------|---------|-----------|----------|
| **Free Consultation** | FREE (20 min) | N/A | Telehealth |
| **Alternative Medicine** | $119 AUD (15 min) | $79 AUD (10 min) | Telehealth |
| **Alt Med In-Person** | $119 AUD (20 min) | $79 AUD (15 min) | Melbourne |
| **GAPS Coaching** | $195 AUD (60 min) | $79 AUD (15 min) | Telehealth |

## 🚀 Migration Path

### Week 1
- [ ] Create database schema
- [ ] Add practitioner records
- [ ] Update API endpoints

### Week 2  
- [ ] Setup Calendly calendars
- [ ] Configure webhooks
- [ ] Test booking flows

### Week 3
- [ ] Build UI components
- [ ] Implement phone booking
- [ ] Admin interface updates

### Week 4
- [ ] Integration testing
- [ ] Staff training
- [ ] Soft launch prep

## 🎯 Success Metrics

### Must Work Before Launch
1. Free consultation bookings creating triage sessions
2. Phone booking by consultants working
3. All 6 practitioner calendars syncing
4. Payment recording over phone
5. MediRecords sync for all practitioners

### KPIs to Track
- Triage → Booking conversion rate (target: >60%)
- Phone booking success rate (target: >90%)
- Calendar sync reliability (target: 99.9%)
- Average time to book after triage (target: <5 min)

## ⚠️ Critical Differences

### Booking Creation
```javascript
// OLD
createBooking({
  doctorId: 'dr-dia', // hardcoded
  payment: 'calendly-stripe', // $89 AUD initial, $69 AUD follow-up
})

// NEW  
createBooking({
  practitionerId: selectedPractitioner, // dynamic
  triageSessionId: required, // must have triage
  payment: 'phone' // recorded manually
})
```

### Webhook Routing
```javascript
// OLD
handleWebhook(event) {
  createBooking(event); // simple
}

// NEW
handleWebhook(event) {
  if (isTriageCalendar(event)) {
    createTriageSession(event);
  } else {
    routeToPractitioner(event);
  }
}
```

## 📱 Phone Booking Process

1. **Consultant receives triage session**
2. **Conducts assessment call**
3. **Opens phone booking interface**
4. **Selects practitioner & time**
5. **Collects payment over phone**
6. **Confirms booking in system**
7. **System syncs to Calendly & MediRecords**

## 🔐 New Permissions

### Consultant Role
```javascript
permissions: [
  'triage:conduct',
  'booking:create_phone',
  'payment:record',
  'practitioner:view_availability'
]
```

### Admin Enhancements  
```javascript
permissions: [
  'calendar:manage_all',
  'practitioner:manage',
  'reports:multi_practitioner'
]
```

---

**Remember:** The core change is adding a FREE triage layer before paid bookings, enabling multi-practitioner support and phone booking capabilities.