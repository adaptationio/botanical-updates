# Service Expansion Overview

## Current Services (New-Update)
- Alternative Medicine (Doctors & Nurse Practitioner)
- GAPS Diet Coaching (GAPS Coach)

## New Services to Add
1. **Weight Loss Program** (Ready to launch)
2. **Counseling Services** (Coming soon)
3. **Equine Therapy** (Coming soon)

## Service Presentation Architecture

### Homepage Service Display
```
BOTANIQAL SERVICES
├── Alternative Medicine ✓
├── GAPS Diet Coaching ✓
├── Weight Loss Program ✓
├── Counseling Services (Coming Soon)
└── Equine Therapy (Coming Soon)
```

### Service-Specific Funnels
Each service will have dedicated landing pages:
- `/alternative-medicine` → Existing flow
- `/gaps-coaching` → Existing flow
- `/weight-loss` → New funnel
- `/counseling` → Coming soon page → Waitlist
- `/equine-therapy` → Coming soon page → Waitlist

## Booking Flow Updates

### Initial Consultation Updates
1. Patient selects area of interest during free consultation booking
2. Options presented:
   - Alternative Medicine
   - GAPS Diet Coaching
   - Weight Loss
   - Counseling (if available)
   - Equine Therapy (if available)
   - Not Sure / General Wellness

### Consultant Triage Updates
- Reviews patient's selected interest
- Still performs comprehensive assessment
- May recommend different or additional services
- Books appropriate practitioner and service type

### Follow-up Booking Updates
- Combined calendar now includes service type selection
- Flow: Select Service → Select Practitioner → Select Time
- Different durations and prices per service type

## Appointment Type Matrix

### Service Types & Durations
| Service | Initial Consult | Follow-up | Duration | Price |
|---------|----------------|-----------|----------|-------|
| Alternative Medicine | Free (with consultant) | $69 | 30 min | Standard |
| GAPS Coaching | Free (with consultant) | $69 | 45 min | Standard |
| Weight Loss | Free (with consultant) | $89 | 45 min | Premium |
| Counseling | Free (with consultant) | $99 | 60 min | Premium |
| Equine Therapy | Free (with consultant) | $129 | 90 min | Specialty |

### Practitioner Specializations
- **Doctor 1**: Alternative Medicine, Weight Loss
- **Doctor 2**: Alternative Medicine, Counseling supervision
- **Nurse Practitioner**: Alternative Medicine, Weight Loss
- **GAPS Coach**: GAPS Diet, Weight Loss support
- **Counselor** (new): Counseling services
- **Equine Therapist** (new): Equine therapy
- **Consultant**: All initial assessments

## Technical Implementation

### Calendly Setup
Each service type needs separate appointment types:
- `initial-consult-free` (all services)
- `followup-alternative-30min`
- `followup-gaps-45min`
- `followup-weightloss-45min`
- `followup-counseling-60min`
- `followup-equine-90min`

### MediRecords Configuration
- Create appointment types matching Calendly
- Set duration and billing codes per type
- Configure practitioner availability per service
- Enable service-based reporting

### Form Updates
- Mini intake: Add service interest dropdown
- Full intake: Service-specific sections
- Dynamic triage: Service-specific question paths

## Variation Timelines

### Phase 1: Coming Soon (Current State)
- Weight Loss: Fully operational
- Counseling: Coming soon page with waitlist
- Equine Therapy: Coming soon page with waitlist

### Phase 2: Fully Operational
- All services active
- 6 practitioners total
- Full appointment type matrix
- Complete funnel ecosystem