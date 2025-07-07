# New Update Comprehensive Booking System Flow

## Overview
This flowchart shows the complete booking ecosystem with all stakeholders: Patient, Consultant, Practitioners (Doctors/Nurse/GAPS Coach), Admin, and Technical Systems for the new multi-practitioner model with free initial consultations.

```mermaid
flowchart TB
    subgraph Patient["Patient Journey"]
        P1[Visit Website] --> P2{Consultation Type}
        P2 -->|Initial - FREE| P3[Select Free Consultation]
        P2 -->|Follow-up| P4[View Combined Calendar]
        
        P3 --> P5[Book with Consultant]
        P5 --> P6[No Payment Required]
        P6 --> P7[Complete Mini Intake + Consent]
        
        P7 --> P8[Receive Call from Consultant]
        P8 --> P9[Answer Triage Questions]
        P9 --> P10[Receive Practitioner Recommendation]
        P10 --> P11[Book & Pay Over Phone]
        P11 --> P12[Complete Full Intake]
        P12 --> P13[Attend Practitioner Appointment]
        
        P4 --> P14[Filter by Practitioner Tab]
        P14 --> P15[Select Time & Practitioner]
        P15 --> P16[Pay Follow-up Fee]
        P16 --> P17[Attend Follow-up]
    end
    
    subgraph Consultant["Consultant Workflow"]
        C1[Review Free Booking] --> C2[Check Mini Intake]
        C2 --> C3[Call Patient]
        C3 --> C4[Conduct Triage Assessment]
        C4 --> C5[Use Dynamic Question Form]
        C5 --> C6{Determine Best Practitioner}
        
        C6 -->|Medical| C7[Recommend Doctor/Nurse]
        C6 -->|Lifestyle| C8[Recommend GAPS Coach]
        C6 -->|Not Suitable| C9[Provide Alternatives]
        
        C7 --> C10[Check Practitioner Availability]
        C8 --> C10
        C10 --> C11[Book Appointment by Phone]
        C11 --> C12[Process Payment]
        C12 --> C13[Complete Triage Notes]
        C13 --> C14[Prepare Handover]
    end
    
    subgraph Practitioners["Practitioners (Doctors/Nurse/GAPS)"]
        PR1[Receive Booking] --> PR2[Review Triage Handover]
        PR2 --> PR3[Review Full Intake]
        PR3 --> PR4[Conduct Appointment]
        PR4 --> PR5{Treatment Decision}
        
        PR5 -->|Prescription| PR6[eRx/SafeScript]
        PR5 -->|Referral| PR7[Internal/External Referral]
        PR5 -->|Follow-up| PR8[Book Next Appointment]
        
        PR6 --> PR9[Complete Notes]
        PR7 --> PR9
        PR8 --> PR9
        PR9 --> PR10[Update MediRecords]
    end
    
    subgraph Admin["Admin Operations"]
        A1[Monitor All Bookings] --> A2{Booking Type}
        A2 -->|Free Consult| A3[Track Consultant Calendar]
        A2 -->|Practitioner| A4[Track 5 Calendars]
        
        A3 --> A5[Verify Mini Intake]
        A4 --> A6[Verify Full Intake]
        
        A5 --> A7[Monitor Phone Bookings]
        A6 --> A8[Process Forms]
        
        A7 --> A9[Manual Entry if Needed]
        A8 --> A10[Update Patient Records]
        
        A9 --> A11[Sync Availability]
        A10 --> A11
        A11 --> A12[Generate Reports]
    end
    
    subgraph Tech["Technical Systems"]
        T1[Webflow CMS] --> T2{Booking Path}
        T2 -->|Free| T3[Consultant Calendly]
        T2 -->|Paid| T4[Combined Calendar View]
        
        T3 --> T5[No Stripe Payment]
        T4 --> T6[Tab-based Filtering]
        T6 --> T7[5 Practitioner Calendly]
        T7 --> T8[Stripe Payment]
        
        T5 --> T9[Office 365 Webhook]
        T8 --> T9
        
        T9 --> T10[AWS Lambda]
        T10 --> T11[Process Booking]
        T11 --> T12[MediRecords API]
        T12 --> T13[Create/Update Patient]
        
        T14[JotForm Mini] --> T15[AWS Lambda]
        T16[JotForm Full] --> T15
        T17[Dynamic Triage Form] --> T15
        
        T15 --> T18[Process Forms]
        T18 --> T12
        
        T19[Phone Booking] --> T20[Manual MediRecords Entry]
        T20 --> T12
    end
    
    %% Key Integration Points
    P5 -.-> T3
    P7 -.-> T14
    P8 <-.-> C3
    P11 -.-> T19
    P12 -.-> T16
    P15 -.-> T7
    
    C1 <-.-> T12
    C5 -.-> T17
    C11 -.-> T19
    C14 -.-> PR2
    
    PR1 <-.-> T12
    PR10 -.-> T12
    
    A1 <-.-> T9
    A7 <-.-> T19
    A9 -.-> T20
    
    %% Styling
    classDef patient fill:#e3f2fd,stroke:#1565c0,stroke-width:3px
    classDef consultant fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    classDef practitioner fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef admin fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef tech fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef free fill:#c8e6c9,stroke:#1b5e20,stroke-width:3px
    
    class P1,P2,P3,P4,P5,P6,P7,P8,P9,P10,P11,P12,P13,P14,P15,P16,P17 patient
    class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C14 consultant
    class PR1,PR2,PR3,PR4,PR5,PR6,PR7,PR8,PR9,PR10 practitioner
    class A1,A2,A3,A4,A5,A6,A7,A8,A9,A10,A11,A12 admin
    class T1,T2,T3,T4,T5,T6,T7,T8,T9,T10,T11,T12,T13,T14,T15,T16,T17,T18,T19,T20 tech
    class P3,P6,T3,T5,C1,C2,C3 free
```

## Key Features of New System

### 1. **Free Initial Consultation Path**
- Patient books free consultation with consultant
- No payment required upfront
- Mini intake + consent form
- Consultant conducts triage
- Phone booking with payment for practitioner

### 2. **Multi-Practitioner Environment**
- 2 Doctors
- 1 Nurse Practitioner
- 1 GAPS Coach
- 1 Consultant (for triage)
- Each with dedicated Calendly calendar

### 3. **Enhanced Booking Options**
- Combined calendar view for follow-ups
- Tab-based filtering by practitioner
- Direct booking for returning patients
- Phone booking for new patients after triage

### 4. **Improved Information Flow**
- Triage assessment captures detailed info
- Handover from consultant to practitioner
- Dynamic forms adapt to patient needs
- Complete patient history available

### 5. **Technical Enhancements**
- 5 separate Calendly integrations
- Dynamic form processing
- Phone booking support
- Enhanced webhook processing
- Multiple form types (mini, full, dynamic)

## Integration Points

### Patient ↔ Consultant
- Phone consultation for triage
- Practitioner recommendation
- Phone booking assistance

### Consultant ↔ Practitioner
- Triage notes handover
- Patient context transfer
- Treatment recommendations

### Systems ↔ Admin
- Booking notifications
- Form submissions
- Manual interventions
- Availability sync

### Technical ↔ Clinical
- Automated data flow
- Manual backup processes
- Real-time updates
- Error handling

## Process Flows

### Initial Consultation (Free)
1. Patient books free slot with consultant
2. Completes mini intake + consent
3. Consultant calls for triage
4. Consultant books practitioner appointment
5. Patient pays over phone
6. Completes full intake
7. Attends practitioner appointment

### Follow-up Consultation
1. Patient views combined calendar
2. Filters by preferred practitioner
3. Selects time and books
4. Pays online
5. Attends appointment

### Manual Interventions
- Admin monitors all bookings
- Syncs availability across systems
- Handles phone booking entries
- Manages form processing issues
- Generates required reports