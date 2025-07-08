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
| Service | Initial Consult | Follow-up | Initial Duration | Follow-up Duration |
|---------|----------------|-----------|------------------|-------------------|
| Free Consult (Consultant) | FREE | N/A | 20 min | N/A |
| Alternative Medicine | $119 | $79 | 15 min | 10 min |
| GAPS Coaching | $195 | $79 | 60 min | 15 min |
| Weight Loss | TBD | TBD | TBD | TBD |
| Counseling (Online) | TBD | TBD | TBD | TBD |
| Equine Therapy | TBD | TBD | TBD | TBD |
| In-Person (Dr Shivani - Melbourne) | $119 | $79 | 20 min | 15 min |

### Practitioner Specializations
- **Doctor 1**: Alternative Medicine (Telehealth)
- **Doctor 2**: Alternative Medicine (Telehealth)
- **Dr Shivani**: In-Person consultations (Melbourne clinic)
- **Nurse Practitioner**: Alternative Medicine (Telehealth)
- **GAPS Coach**: GAPS Diet coaching
- **Counselor** (new): Online counseling services
- **Equine Therapist** (new): Equine therapy
- **Consultant**: Free initial assessments (20 min)

## Technical Implementation

### Calendly Setup
Each service type needs separate appointment types:
- `free-consult-20min` (consultant triage)
- `initial-alternative-15min` ($119)
- `followup-alternative-10min` ($79)
- `initial-gaps-60min` ($195)
- `followup-gaps-15min` ($79)
- `initial-inperson-20min` ($119)
- `followup-inperson-15min` ($79)
- Weight loss types (TBD)
- Counseling types (TBD)
- Equine therapy types (TBD)

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