# Service Triage Process - Consultant Assessment

## Overview
This diagram details how the consultant (currently GAPS Coach) assesses patients and matches them with appropriate services and practitioners.

```mermaid
graph TD
    Start([Triage Call Begins]) --> Initial[Initial Greeting]
    
    Initial --> Reason{Primary Concern?}
    
    %% Primary Concern Categories
    Reason -->|Physical Health| Physical[Physical Assessment]
    Reason -->|Mental Health| Mental[Mental Health Screen]
    Reason -->|Weight Issues| Weight[Weight Assessment]
    Reason -->|Digestive/Gut| Digestive[GAPS Assessment]
    Reason -->|Multiple| Multi[Multi-Service Review]
    
    %% Physical Health Path
    Physical --> PhysicalQ[Medical History Questions]
    PhysicalQ --> Complexity{Complexity Level}
    
    Complexity -->|Simple| SimpleAlt[Alt Medicine - Nurse]
    Complexity -->|Moderate| ModAlt[Alt Medicine - Doctor]
    Complexity -->|Complex| ComplexAlt[Alt Medicine - Dr. Shivani]
    
    %% Mental Health Path
    Mental --> MentalQ[Mental Health Questions]
    MentalQ --> MentalType{Service Need}
    
    MentalType -->|Counseling| CounselRec[Recommend Counselor]
    MentalType -->|Holistic| HolisticRec[Alt Med + Counseling]
    MentalType -->|Trauma| TraumaRec[Consider Equine Therapy]
    
    %% Weight Path
    Weight --> WeightQ[Weight History Questions]
    WeightQ --> WeightApproach{Best Approach}
    
    WeightApproach -->|Medical| WeightMed[Weight Loss - Doctor]
    WeightApproach -->|Nutrition| WeightNut[Weight Loss + GAPS]
    WeightApproach -->|Emotional| WeightEmo[Weight Loss + Counseling]
    
    %% Digestive Path
    Digestive --> DigestQ[Digestive Health Questions]
    DigestQ --> GAPSFit{GAPS Suitable?}
    
    GAPSFit -->|Yes| GAPSYes[GAPS Coach Recommended]
    GAPSFit -->|Maybe| GAPSMaybe[GAPS + Alt Medicine]
    GAPSFit -->|No| GAPSNo[Alt Medicine Only]
    
    %% Multi-Service Path
    Multi --> Priority[Prioritize Needs]
    Priority --> Integration[Create Care Plan]
    Integration --> Team[Build Care Team]
    
    %% All paths converge
    SimpleAlt --> FormSelect[Select Intake Form]
    ModAlt --> FormSelect
    ComplexAlt --> FormSelect
    CounselRec --> FormSelect
    HolisticRec --> FormSelect
    TraumaRec --> FormSelect
    WeightMed --> FormSelect
    WeightNut --> FormSelect
    WeightEmo --> FormSelect
    GAPSYes --> FormSelect
    GAPSMaybe --> FormSelect
    GAPSNo --> FormSelect
    Team --> FormSelect
    
    %% Form Selection
    FormSelect --> FormChoice{Which Form?}
    
    FormChoice --> F1[Medical History Form]
    FormChoice --> F2[Weight Loss Form]
    FormChoice --> F3[GAPS Nutrition Form]
    FormChoice --> F4[Mental Health Form]
    FormChoice --> F5[Multi-Service Form]
    
    %% Complete Assessment
    F1 --> Complete[Complete During Call]
    F2 --> Complete
    F3 --> Complete
    F4 --> Complete
    F5 --> Complete
    
    Complete --> Recommend[Final Recommendations]
    Recommend --> BookingProcess[Proceed to Phone Booking]
    
    %% Styling
    classDef assess fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef health fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef mental fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef weight fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef gaps fill:#e0f2f1,stroke:#00796b,stroke-width:2px
    classDef form fill:#ffebee,stroke:#c62828,stroke-width:2px
    
    class Initial,Reason assess
    class Physical,PhysicalQ,SimpleAlt,ModAlt,ComplexAlt health
    class Mental,MentalQ,CounselRec,HolisticRec,TraumaRec mental
    class Weight,WeightQ,WeightMed,WeightNut,WeightEmo weight
    class Digestive,DigestQ,GAPSYes,GAPSMaybe,GAPSNo gaps
    class F1,F2,F3,F4,F5 form
```

## Triage Decision Framework

### Initial Assessment Questions

#### 1. Primary Concern Identification
- "What brings you to us today?"
- "What's your main health concern?"
- "How long has this been an issue?"
- "What have you tried so far?"

#### 2. Service Matching Criteria

**Alternative Medicine Match:**
- General health concerns
- Chronic conditions
- Preventive care
- Natural treatment preference

**GAPS Coaching Match:**
- Digestive issues
- Autism/ADHD
- Autoimmune conditions
- Failed conventional diets

**Weight Loss Match:**
- BMI concerns
- Failed diet attempts
- Medical weight issues
- Lifestyle change needs

**Counseling Match:**
- Mental health concerns
- Stress/anxiety
- Relationship issues
- Behavioral changes

**Equine Therapy Match:**
- Children with special needs
- Trauma recovery
- Alternative therapy interest
- Failed traditional therapy

### Practitioner Assignment Logic

#### For Alternative Medicine & Weight Loss

| Complexity | Assigned To | Factors |
|------------|-------------|---------|
| **Simple** | Nurse Practitioner | Routine care, stable conditions |
| **Moderate** | Doctor 1 | Multiple conditions, medication management |
| **Complex** | Dr. Shivani | Chronic complex cases, prefer in-person option |

#### Location Considerations
- **Telehealth Only**: Doctor 1, Nurse
- **Telehealth + Melbourne**: Dr. Shivani
- Patient preference considered
- Availability factored

### Dynamic Form Selection

The consultant selects forms based on:

1. **Single Service Interest** → Service-specific form
2. **Multiple Concerns** → Multi-service comprehensive form
3. **Complex History** → Extended medical history form
4. **Mental Health Component** → Include mental health screening
5. **Family Involvement** → Family intake forms

### Multi-Service Care Plans

For patients needing multiple services:

1. **Identify Primary Need** - What's most urgent?
2. **Check Dependencies** - What needs to happen first?
3. **Plan Sequence** - Logical order of services
4. **Coordinate Practitioners** - Ensure communication
5. **Set Timeline** - Realistic scheduling

Example Care Plans:

**Weight + Mental Health:**
- Start with counseling for emotional eating
- Add weight loss program after 2-4 weeks
- Coordinate between counselor and doctor

**GAPS + Alternative Medicine:**
- Begin with medical assessment
- Start GAPS protocol with coach
- Monthly check-ins with doctor

## Quality Assurance

### Triage Checkpoints
- ✓ All major concerns addressed
- ✓ Appropriate service selected
- ✓ Right practitioner matched
- ✓ Patient agrees with plan
- ✓ Booking details confirmed

### When to Escalate
- Complex medical history
- Multiple failed treatments
- Urgent care needs
- Outside scope of practice
- Patient dissatisfaction

### Documentation Requirements
- Summary of concerns
- Services recommended
- Practitioner assigned
- Reason for selection
- Alternative options discussed

[← Back to Overview](./patient-booking-overview.md) | [Next: Phone Booking Process →](./phone-booking-process.md)