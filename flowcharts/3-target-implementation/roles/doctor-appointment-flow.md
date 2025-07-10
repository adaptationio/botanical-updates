# Doctor Appointment Flow - Future Options

## Overview
This flowchart shows how doctors manage appointments across multiple services (Alternative Medicine, Weight Loss) with cross-service referral capabilities.

```mermaid
graph TD
    Start([Appointment Start]) --> CheckType{Appointment Type}
    
    CheckType -->|Alt Medicine| AltMedFlow[Alternative Medicine<br/>15-20 min]
    CheckType -->|Weight Loss| WeightFlow[Weight Loss Program<br/>Duration TBD]
    CheckType -->|Multi-Service| MultiFlow[Integrated Care<br/>Extended Time]
    
    %% Pre-Appointment Review
    AltMedFlow --> PreReview[Review Patient Info]
    WeightFlow --> PreReview
    MultiFlow --> PreReview
    
    PreReview --> ReviewItems{Review Components}
    
    ReviewItems --> TriageNotes[Consultant Triage Notes]
    ReviewItems --> ServiceHistory[Service History:<br/>• Previous Alt Med<br/>• GAPS Progress<br/>• Weight History]
    ReviewItems --> IntakeForms[Completed Forms:<br/>• Medical History<br/>• Service-Specific<br/>• Multi-Service]
    ReviewItems --> LabResults[Recent Labs/Tests]
    
    %% Join Appointment
    TriageNotes --> JoinCall[Join Video Call]
    ServiceHistory --> JoinCall
    IntakeForms --> JoinCall
    LabResults --> JoinCall
    
    %% Service-Specific Consultations
    JoinCall --> ServiceConsult{Service-Specific Consultation}
    
    %% Alternative Medicine Path
    ServiceConsult -->|Alt Medicine| AltMedConsult[Holistic Assessment]
    AltMedConsult --> AltMedExam[Natural Medicine Evaluation:<br/>• Current Symptoms<br/>• Lifestyle Factors<br/>• Stress Assessment<br/>• Sleep Quality]
    
    AltMedExam --> AltMedPlan{Treatment Planning}
    
    AltMedPlan --> Natural[Natural Remedies]
    AltMedPlan --> Lifestyle[Lifestyle Changes]
    AltMedPlan --> Referral1[Consider Other Services]
    
    %% Weight Loss Path
    ServiceConsult -->|Weight Loss| WeightConsult[Weight Management Assessment]
    WeightConsult --> WeightExam[Comprehensive Evaluation:<br/>• BMI & Measurements<br/>• Diet History<br/>• Exercise Habits<br/>• Medical Factors]
    
    WeightExam --> WeightPlan{Program Design}
    
    WeightPlan --> Medical[Medical Interventions]
    WeightPlan --> Nutrition[Nutrition Plan]
    WeightPlan --> Exercise[Exercise Protocol]
    WeightPlan --> Referral2[Support Services]
    
    %% Multi-Service Path
    ServiceConsult -->|Multi-Service| MultiConsult[Integrated Assessment]
    MultiConsult --> MultiExam[Comprehensive Review:<br/>• All Active Conditions<br/>• Service Interactions<br/>• Priority Setting<br/>• Care Coordination]
    
    MultiExam --> MultiPlan{Integrated Care Plan}
    
    MultiPlan --> PrimaryTreat[Primary Treatment]
    MultiPlan --> SecondaryTreat[Supporting Services]
    MultiPlan --> Timeline[Treatment Timeline]
    
    %% Cross-Service Referrals
    Referral1 --> CrossRefer{Cross-Service Referral}
    Referral2 --> CrossRefer
    SecondaryTreat --> CrossRefer
    
    CrossRefer -->|Nutrition Need| GAPSRefer[Refer to GAPS Coach]
    CrossRefer -->|Mental Health| CounselRefer[Waitlist for Counseling]
    CrossRefer -->|Weight Support| WeightRefer[Add Weight Loss Program]
    CrossRefer -->|Holistic Need| AltMedRefer[Add Alternative Medicine]
    CrossRefer -->|Special Therapy| EquineRefer[Waitlist for Equine]
    
    %% Documentation
    Natural --> Documentation[Document in MediRecords]
    Lifestyle --> Documentation
    Medical --> Documentation
    Nutrition --> Documentation
    Exercise --> Documentation
    PrimaryTreat --> Documentation
    Timeline --> Documentation
    
    GAPSRefer --> ReferralDoc[Document Referrals]
    CounselRefer --> ReferralDoc
    WeightRefer --> ReferralDoc
    AltMedRefer --> ReferralDoc
    EquineRefer --> ReferralDoc
    
    %% Follow-up Planning
    Documentation --> FollowUpPlan{Follow-up Planning}
    ReferralDoc --> FollowUpPlan
    
    FollowUpPlan --> SameService[Continue Same Service]
    FollowUpPlan --> AddService[Add New Service]
    FollowUpPlan --> Transition[Transition Care]
    FollowUpPlan --> Discharge[Treatment Complete]
    
    %% Booking Next Steps
    SameService --> BookFollowUp[Book Follow-up:<br/>10-15 min slots]
    AddService --> CoordinateCare[Coordinate Multi-Service]
    Transition --> HandoffProcess[Handoff to Specialist]
    
    %% Waitlist Handling
    CounselRefer --> WaitlistProcess[Add to Waitlist:<br/>• Note current treatment<br/>• Priority level<br/>• Integration needs]
    EquineRefer --> WaitlistProcess
    
    %% Complete Appointment
    BookFollowUp --> CompleteVisit[Complete Appointment]
    CoordinateCare --> CompleteVisit
    HandoffProcess --> CompleteVisit
    Discharge --> CompleteVisit
    WaitlistProcess --> CompleteVisit
    
    CompleteVisit --> End([Appointment End])
    
    %% Styling
    classDef service fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    classDef assess fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef refer fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef waitlist fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef doc fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    
    class AltMedFlow,WeightFlow,MultiFlow service
    class AltMedConsult,WeightConsult,MultiConsult,AltMedExam,WeightExam assess
    class CrossRefer,GAPSRefer,WeightRefer,AltMedRefer refer
    class CounselRefer,EquineRefer,WaitlistProcess waitlist
    class Documentation,ReferralDoc,CompleteVisit doc
```

## Service-Specific Protocols

### Alternative Medicine Appointments

**Duration**: 15-20 minutes
**Focus Areas**:
- Natural health assessment
- Holistic treatment planning
- Lifestyle medicine
- Preventive care

**Common Interventions**:
- Herbal medicine recommendations
- Nutritional supplements
- Stress management techniques
- Mind-body practices

**Typical Referrals**:
- GAPS Coach for gut health
- Weight Loss for BMI concerns
- Counseling waitlist for mental health

### Weight Loss Program Appointments

**Duration**: TBD (likely 30-45 min initial)
**Focus Areas**:
- Medical weight assessment
- Metabolic evaluation
- Behavioral factors
- Sustainable planning

**Program Components**:
- Medical clearance
- Prescription options
- Nutrition planning
- Exercise prescription
- Behavioral support

**Common Referrals**:
- GAPS Coach for nutrition
- Counseling waitlist for emotional eating
- Alternative Medicine for holistic support

## Cross-Service Referral Matrix

| From Service | To Service | Common Reasons | Process |
|--------------|------------|----------------|---------|
| Alt Medicine | GAPS | Digestive issues, inflammation | Warm handoff to GAPS Coach |
| Alt Medicine | Weight Loss | BMI concerns, metabolic health | Add to doctor's weight program |
| Alt Medicine | Counseling | Stress, anxiety, depression | Add to counseling waitlist |
| Weight Loss | GAPS | Nutrition optimization | Coordinate with GAPS Coach |
| Weight Loss | Counseling | Emotional eating patterns | Priority waitlist placement |
| Weight Loss | Alt Medicine | Holistic support needed | Book with same/other doctor |

## Multi-Service Care Coordination

### Integrated Appointments
For patients in multiple programs:

1. **Extended Time Slots**
   - Review all services
   - Check interactions
   - Unified planning
   - Coordinated goals

2. **Service Priority Matrix**
   ```
   Primary Condition → Lead Service
   Supporting Needs → Secondary Services
   Preventive Care → Maintenance Services
   ```

3. **Documentation Integration**
   - Shared care plans
   - Cross-service notes
   - Unified progress tracking
   - Team communication

### Care Team Communication

**With GAPS Coach:**
- Nutrition plans alignment
- Supplement coordination
- Dietary restrictions
- Progress sharing

**With Other Doctors:**
- Shared patients
- Treatment coordination
- Medication management
- Referral updates

**With Waitlisted Services:**
- Priority flagging
- Current treatment notes
- Integration planning
- Launch notifications

## Service-Specific Documentation

### Alternative Medicine Notes
```
Chief Complaint: [Natural health concern]
Assessment: [Holistic evaluation]
Plan: 
- Natural interventions
- Lifestyle modifications
- Follow-up schedule
- Referrals if needed
```

### Weight Loss Notes
```
Current Weight/BMI: [Metrics]
Program Phase: [Initial/Active/Maintenance]
Interventions:
- Medical: [If applicable]
- Nutrition: [Plan summary]
- Exercise: [Prescription]
- Behavioral: [Strategies]
Progress: [Tracking metrics]
Next Steps: [Follow-up plan]
```

## Follow-up Scheduling Logic

### Within Same Service
- Alternative Medicine: 10-15 min follow-ups
- Weight Loss: Duration based on phase
- Standard intervals: 2-4 weeks

### Adding Services
1. Check availability across services
2. Coordinate timing
3. Avoid appointment overload
4. Consider patient capacity

### Transitioning Care
- Complete current treatment phase
- Warm handoff to next provider
- Ensure continuity documentation
- Follow-up as needed

## Quality Metrics

### Appointment Efficiency
- On-time starts: >95%
- Duration adherence: ±5 minutes
- Documentation time: <5 minutes post-visit

### Cross-Service Metrics
- Referral completion rate
- Multi-service retention
- Integrated care outcomes
- Patient satisfaction

### Waitlist Management
- Appropriate waitlist adds
- Priority flagging accuracy
- Follow-through tracking
- Launch conversion prep

[← Back to Consultant Flow](./consultant-triage-flow.md) | [Next: Tech Architecture →](./tech-architecture-flow.md)