# New Update Doctor Appointment Flow

## Overview
This flowchart represents the updated doctor workflow with consultant triage handover and multi-practitioner environment.

```mermaid
flowchart TD
    %% Pre-Appointment - Enhanced
    Start([Appointment Time]) --> OpenMediRecords[Open MediRecords]
    OpenMediRecords --> CheckType{Appointment Type}
    
    CheckType -->|Post-Triage| ReviewTriage[Review Triage Notes]
    CheckType -->|Follow-up| ReviewHistory[Review Patient History]
    
    ReviewTriage --> ReadHandover[Read Consultant Handover]
    ReadHandover --> TriageInsights[Review Triage Recommendations]
    TriageInsights --> PatientPhone[Get Patient Phone Number]
    
    ReviewHistory --> PreviousNotes[Check Previous Notes]
    PreviousNotes --> PatientPhone
    
    %% During Appointment
    PatientPhone --> CallPatient[Call Patient]
    CallPatient --> VerifyIdentity[Verify Identity]
    
    VerifyIdentity --> AppointmentFlow{Consultation Type}
    
    AppointmentFlow -->|New from Triage| TriageFollowUp[Follow Triage Plan]
    AppointmentFlow -->|Regular Follow-up| StandardConsult[Standard Consultation]
    
    %% Triage Follow-up Path
    TriageFollowUp --> ConfirmTriage[Confirm Triage Assessment]
    ConfirmTriage --> ExpandAssessment[Expand on Triage Findings]
    ExpandAssessment --> MedicalExam[Detailed Medical Assessment]
    
    %% Standard Path
    StandardConsult --> CurrentStatus[Assess Current Status]
    CurrentStatus --> ProgressReview[Review Progress]
    ProgressReview --> MedicalExam
    
    %% Medical Assessment
    MedicalExam --> CheckMedHistory[Review Medical History]
    CheckMedHistory --> SafeScript[Check SafeScript]
    SafeScript --> RequireInfo{Additional Info Needed?}
    
    RequireInfo -->|Yes| RequestInfo[Request from Patient]
    RequireInfo -->|No| TreatmentPlan[Develop Treatment Plan]
    
    RequestInfo --> UpdateNotes[Update Notes with Info]
    UpdateNotes --> TreatmentPlan
    
    %% Treatment Decision
    TreatmentPlan --> TreatmentType{Treatment Type}
    
    TreatmentType -->|Medication| PrescriptionProcess[Prescription Process]
    TreatmentType -->|Referral| ReferralProcess[Referral Process]
    TreatmentType -->|Lifestyle| LifestyleGuidance[Lifestyle Recommendations]
    TreatmentType -->|Multiple| CombinedTreatment[Combined Approach]
    
    %% Prescription Process
    PrescriptionProcess --> eRxSystem[Access eRx in MediRecords]
    eRxSystem --> CreateScript[Create Electronic Prescription]
    CreateScript --> DeliveryMethod{Delivery Method}
    
    DeliveryMethod -->|SMS| SendSMS[Send via SMS]
    DeliveryMethod -->|Email| SendEmail[Send via Email]
    DeliveryMethod -->|Pharmacy| SendPharmacy[Send to Pharmacy]
    
    %% Referral Process
    ReferralProcess --> ReferralType{Referral Type}
    ReferralType -->|Specialist| SpecialistReferral[Create Specialist Referral]
    ReferralType -->|Allied Health| AlliedReferral[Allied Health Referral]
    ReferralType -->|GAPS Coach| InternalReferral[Refer to Ramona (GAPS Coach)]
    
    %% Follow-up Planning
    SendSMS --> FollowUpPlan[Plan Follow-up]
    SendEmail --> FollowUpPlan
    SendPharmacy --> FollowUpPlan
    SpecialistReferral --> FollowUpPlan
    AlliedReferral --> FollowUpPlan
    InternalReferral --> FollowUpPlan
    LifestyleGuidance --> FollowUpPlan
    CombinedTreatment --> FollowUpPlan
    
    FollowUpPlan --> FollowUpNeeded{Follow-up Required?}
    
    FollowUpNeeded -->|Yes| ScheduleType{Schedule Type}
    FollowUpNeeded -->|No| CompleteNotes[Complete Clinical Notes]
    
    ScheduleType -->|With Me| BookFollowUp[Book Follow-up]
    ScheduleType -->|Other Practitioner| CrossRefer[Cross-refer to Colleague]
    ScheduleType -->|Ramona| CoachRefer[Refer to Ramona (GAPS Coach)]
    
    BookFollowUp --> CompleteNotes
    CrossRefer --> NotifyColleague[Notify Other Practitioner]
    CoachRefer --> NotifyCoach[Notify Ramona]
    
    NotifyColleague --> CompleteNotes
    NotifyCoach --> CompleteNotes
    
    %% Documentation
    CompleteNotes --> FinalizeNotes[Finalize All Notes]
    FinalizeNotes --> SaveRecord[Save Patient Record]
    SaveRecord --> UpdateStatus[Update to Complete]
    
    %% Handover if Needed
    UpdateStatus --> HandoverNeeded{Handover Required?}
    
    HandoverNeeded -->|Yes| PrepareHandover[Prepare Handover Notes]
    HandoverNeeded -->|No| End([End])
    
    PrepareHandover --> ShareWithTeam[Share with Healthcare Team]
    ShareWithTeam --> End
    
    %% Styling
    classDef doctor fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef triage fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    classDef decision fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef prescription fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef referral fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef documentation fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    
    class Start,CallPatient,VerifyIdentity,MedicalExam doctor
    class ReviewTriage,ReadHandover,TriageInsights,TriageFollowUp,ConfirmTriage triage
    class CheckType,AppointmentFlow,RequireInfo,TreatmentType,DeliveryMethod,ReferralType,FollowUpNeeded,ScheduleType,HandoverNeeded decision
    class PrescriptionProcess,eRxSystem,CreateScript prescription
    class ReferralProcess,SpecialistReferral,AlliedReferral,InternalReferral,CrossRefer,CoachRefer referral
    class CompleteNotes,FinalizeNotes,SaveRecord,PrepareHandover documentation
```

## Key Changes in Doctor Workflow

### 1. Triage Integration
- **Consultant Handover**: Review triage assessment before appointment
- **Triage Recommendations**: Follow consultant's suggested care plan
- **Continuity of Care**: Build on initial assessment

### 2. Multi-Practitioner Environment
- **Cross-referral Options**: Can refer to other doctors, nurse, or GAPS coach
- **Team Collaboration**: Share notes with other practitioners
- **Coordinated Care**: Handover process for complex cases

### 3. Enhanced Referral Options
- **Internal Referrals**: Direct to GAPS coach or nurse practitioner
- **External Referrals**: Specialists and allied health
- **Follow-up Flexibility**: Can book with self or recommend colleague

### 4. Improved Documentation
- **Triage Context**: Notes build on initial assessment
- **Handover Notes**: Prepare for team collaboration
- **Shared Records**: Accessible by healthcare team

### 5. Appointment Types
- **Post-Triage**: First appointment after consultant assessment
- **Follow-up**: Regular ongoing appointments
- **Different Workflows**: Tailored approach for each type

## Workflow Variations

### For Post-Triage Appointments
1. Review consultant's triage notes
2. Confirm and expand assessment
3. Implement recommended care plan
4. Consider multi-practitioner approach

### For Follow-up Appointments
1. Review previous notes and progress
2. Standard consultation process
3. Adjust treatment as needed
4. Plan ongoing care

## Collaboration Points
- **With Consultant**: Review triage recommendations
- **With Other Doctors**: Cross-referral for specialty needs
- **With Nurse Practitioner**: Routine care handover
- **With Ramona (GAPS Coach)**: Lifestyle intervention referrals
- **With Admin**: Booking and scheduling support

## Enhanced Features
- Better context from triage process
- Multiple referral pathways
- Team-based care approach
- Flexible follow-up options
- Comprehensive documentation