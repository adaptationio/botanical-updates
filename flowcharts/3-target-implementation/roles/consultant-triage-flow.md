# Consultant Triage Flow - Future Options

## Overview
This flowchart shows how consultants handle multi-service triage, including routing to active services and capturing interest for waitlisted services.

```mermaid
graph TD
    Start([Free Consultation Call]) --> Greeting[Introduction & Overview]
    
    Greeting --> ServiceMenu[Explain Available Services:<br/>✅ Alt Medicine<br/>✅ GAPS<br/>✅ Weight Loss<br/>⏳ Counseling<br/>⏳ Equine Therapy]
    
    ServiceMenu --> InitialScreen{Initial Interest?}
    
    %% Single Service Interest
    InitialScreen -->|Single Service| SingleService[Single Service Path]
    InitialScreen -->|Multiple Services| MultiService[Multi-Service Assessment]
    InitialScreen -->|Unsure| Discovery[Discovery Questions]
    
    %% Discovery Path
    Discovery --> DiscoveryQ[Ask About:<br/>• Health Concerns<br/>• Previous Treatments<br/>• Goals<br/>• Preferences]
    DiscoveryQ --> ServiceMatch[Match to Services]
    ServiceMatch --> ServiceRecommend{Recommend Services}
    
    %% Single Service Paths
    SingleService --> ServiceType{Which Service?}
    
    ServiceType -->|Alt Medicine| AltMedAssess[Medical History Assessment]
    ServiceType -->|GAPS| GAPSAssess[Digestive Health Assessment]
    ServiceType -->|Weight Loss| WeightAssess[Weight History Assessment]
    ServiceType -->|Counseling| CounselWaitlist[Counseling Interest → Waitlist]
    ServiceType -->|Equine| EquineWaitlist[Equine Interest → Waitlist]
    
    %% Multi-Service Assessment
    MultiService --> PriorityAssess[Assess Priority Needs]
    PriorityAssess --> ServiceCombos{Common Combinations}
    
    ServiceCombos --> Combo1[Weight Loss + GAPS<br/>(Nutrition Focus)]
    ServiceCombos --> Combo2[Alt Med + GAPS<br/>(Holistic Health)]
    ServiceCombos --> Combo3[Weight Loss + Counseling<br/>(Emotional Eating)]
    ServiceCombos --> Combo4[Alt Med + Counseling<br/>(Mind-Body)]
    ServiceCombos --> CustomCombo[Custom Combination]
    
    %% Active Service Processing
    AltMedAssess --> FormSelect[Select Appropriate Form]
    GAPSAssess --> FormSelect
    WeightAssess --> FormSelect
    Combo1 --> FormSelect
    Combo2 --> FormSelect
    CustomCombo --> FormSelect
    
    FormSelect --> FormType{Form Selection}
    
    FormType --> MedicalForm[Medical History Form]
    FormType --> NutritionForm[Nutrition Assessment Form]
    FormType --> WeightForm[Weight Management Form]
    FormType --> MultiForm[Multi-Service Form]
    FormType --> ComprehensiveForm[Comprehensive Health Form]
    
    %% Complete Assessment
    MedicalForm --> CompleteForm[Complete During Call]
    NutritionForm --> CompleteForm
    WeightForm --> CompleteForm
    MultiForm --> CompleteForm
    ComprehensiveForm --> CompleteForm
    
    %% Waitlist Processing
    CounselWaitlist --> WaitlistProcess[Waitlist Capture Process]
    EquineWaitlist --> WaitlistProcess
    Combo3 --> MixedService[Active + Waitlist Service]
    Combo4 --> MixedService
    
    WaitlistProcess --> WaitlistInfo[Collect:<br/>• Contact Details<br/>• Interest Level<br/>• Availability<br/>• Specific Needs]
    
    MixedService --> ActiveFirst[Book Active Service]
    ActiveFirst --> WaitlistAdd[Add to Waitlist for Other]
    
    %% Practitioner Matching
    CompleteForm --> MatchPractitioner{Match to Practitioner}
    
    MatchPractitioner -->|Simple Case| AssignNurse[Assign Nurse Practitioner]
    MatchPractitioner -->|Complex Case| AssignDoctor[Assign Doctor]
    MatchPractitioner -->|Location Pref| AssignDrShivani[Assign Dr. Shivani]
    MatchPractitioner -->|GAPS Need| AssignGAPS[Assign Ramona (GAPS Coach)]
    MatchPractitioner -->|Multiple| BuildTeam[Build Care Team]
    
    %% Booking Handoff
    AssignNurse --> BookingProcess[Phone Booking Process]
    AssignDoctor --> BookingProcess
    AssignDrShivani --> BookingProcess
    AssignGAPS --> BookingProcess
    BuildTeam --> BookingProcess
    WaitlistInfo --> WaitlistConfirm[Confirm Waitlist Entry]
    WaitlistAdd --> BookingProcess
    
    BookingProcess --> ScheduleAppt[Schedule Appointment(s)]
    WaitlistConfirm --> FollowUp[Waitlist Follow-up Plan]
    
    %% Completion
    ScheduleAppt --> End([Consultation Complete])
    FollowUp --> End
    
    %% Styling
    classDef active fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef waitlist fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef assess fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef form fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef booking fill:#e0f2f1,stroke:#00796b,stroke-width:2px
    
    class AltMedAssess,GAPSAssess,WeightAssess,Combo1,Combo2 active
    class CounselWaitlist,EquineWaitlist,WaitlistProcess,WaitlistInfo waitlist
    class InitialScreen,Discovery,PriorityAssess assess
    class FormSelect,MedicalForm,NutritionForm,WeightForm form
    class BookingProcess,ScheduleAppt booking
```

## Multi-Service Triage Process

### Service Status Communication
**Opening Script Elements:**
- "We currently offer Alternative Medicine, GAPS Coaching, and our new Weight Loss program"
- "We're also building interest lists for Counseling and Equine Therapy"
- "Let me understand your needs to recommend the best service path"

### Assessment Strategies

#### Single Service Interest
**Active Services (Alt Med, GAPS, Weight Loss):**
1. Focused assessment questions
2. Service-specific form selection
3. Direct practitioner matching
4. Immediate booking process

**Waitlisted Services (Counseling, Equine):**
1. Capture detailed interest
2. Explain timeline expectations
3. Collect contact preferences
4. Offer alternative active services

#### Multi-Service Interest
**Common Combinations:**

| Combination | Approach | Sequence |
|-------------|----------|----------|
| Weight Loss + GAPS | Nutrition-focused plan | Start with GAPS assessment |
| Alt Med + GAPS | Holistic health approach | Medical clearance first |
| Weight Loss + Counseling | Address emotional eating | Book weight loss, waitlist counseling |
| Alt Med + Counseling | Mind-body wellness | Book alt med, waitlist counseling |

### Dynamic Form Selection Logic

```
IF single_service AND active:
    SELECT service_specific_form
ELSE IF multiple_services AND all_active:
    SELECT multi_service_form
ELSE IF multiple_services AND mixed_status:
    SELECT comprehensive_form + waitlist_capture
ELSE IF single_service AND waitlisted:
    SELECT interest_capture_form
```

## Waitlist Management Process

### Waitlist Capture Requirements
1. **Basic Information**
   - Full name and contact details
   - Preferred contact method
   - Best times to reach

2. **Service Interest**
   - Specific service desired
   - Reason for interest
   - Timeline expectations
   - Budget considerations

3. **Qualifying Questions**
   - Previous therapy experience (Counseling)
   - Comfort with animals (Equine)
   - Geographic location
   - Availability preferences

### Waitlist Communication
**Immediate:**
- Confirm addition to waitlist
- Set realistic expectations
- Provide estimated timeline
- Offer alternative services

**Follow-up:**
- Weekly interest updates
- Service launch announcements
- Early bird opportunities
- Educational content

## Practitioner Matching Criteria

### For Active Services

**Complexity Assessment:**
- **Simple**: Single condition, stable → Nurse Practitioner
- **Moderate**: Multiple conditions, medications → Dr Dia
- **Complex**: Chronic conditions, prefer in-person → Dr. Shivani
- **Specialized**: Gut health focus → Ramona (GAPS Coach)

**Multi-Service Matching:**
1. Identify primary service need
2. Check practitioner availability
3. Consider care coordination
4. Build integrated schedule

### For Mixed Active/Waitlist

**Booking Strategy:**
1. Book all available active services
2. Add to relevant waitlists
3. Note connection in records
4. Plan integrated care when available

## Quality Metrics

### Triage Effectiveness
- Service match accuracy
- Patient satisfaction
- Conversion rates
- Waitlist engagement

### Key Performance Indicators
- Average call duration: 20 minutes
- Service identification: <5 minutes
- Form completion: 10-12 minutes
- Booking handoff: 3-5 minutes

### Documentation Requirements
**For Active Services:**
- Selected service(s)
- Completed form type
- Practitioner assignment
- Special considerations

**For Waitlisted Services:**
- Interest level (1-10)
- Specific needs noted
- Contact preferences
- Alternative services offered

## Consultant Tools & Resources

### Quick Reference Guides
- Service comparison chart
- Common conditions mapping
- Practitioner specialties
- Form selection matrix

### Scripts & Templates
- Service introductions
- Waitlist explanations
- Objection handling
- Booking transitions

### System Access
- Calendly (view all practitioners)
- CRM (patient records)
- Waitlist management tool
- Form library

[← Back to Admin Flow](./admin-dashboard-flow.md) | [Next: Doctor Flow →](./doctor-appointment-flow.md)