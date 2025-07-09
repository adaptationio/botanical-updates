# Returning Patient Flow - Follow-up Booking

## Overview
This diagram shows how existing patients book follow-up appointments with direct online access.

```mermaid
graph TD
    Start([Returning Patient]) --> Access{Access Method}
    
    %% Access Points
    Access -->|Website| Homepage[Homepage Link]
    Access -->|Email| EmailLink[Follow-up Reminder]
    Access -->|SMS| SMSLink[Text Message Link]
    
    %% All lead to follow-up page
    Homepage --> FollowPage[Follow-up Booking Page]
    EmailLink --> FollowPage
    SMSLink --> FollowPage
    
    %% Service Selection
    FollowPage --> SelectService{Select Service}
    
    SelectService --> AltMedFollow[Alternative Medicine]
    SelectService --> GAPSFollow[GAPS Coaching]
    SelectService --> WeightFollow[Weight Loss]
    SelectService --> CounselFollow[Counseling]
    SelectService --> EquineFollow[Equine Therapy]
    
    %% Show Relevant Practitioners
    AltMedFollow --> AltMedCal[Combined Calendar:<br/>Dr 1, Dr Shivani, Nurse]
    GAPSFollow --> GAPSCal[GAPS Coach Calendar]
    WeightFollow --> WeightCal[Combined Calendar:<br/>Dr 1, Dr Shivani, Nurse]
    CounselFollow --> CounselCal[Counselor Calendar]
    EquineFollow --> EquineCal[Therapist Calendar]
    
    %% Calendar View Options
    AltMedCal --> ViewType{View Preference}
    WeightCal --> ViewType
    
    ViewType -->|All Available| AllView[Show All Practitioners]
    ViewType -->|Specific| FilterView[Filter by Practitioner]
    
    GAPSCal --> TimeSelect[Select Time Slot]
    CounselCal --> TimeSelect
    EquineCal --> TimeSelect
    AllView --> TimeSelect
    FilterView --> TimeSelect
    
    %% Booking Completion
    TimeSelect --> ReviewDetails[Review Appointment]
    ReviewDetails --> Payment[Online Payment]
    
    Payment --> Card[Credit Card Payment via Stripe]
    
    Card --> Confirmed[Booking Confirmed]
    
    %% Confirmation
    Confirmed --> Notifications[Email + SMS Confirmation]
    Notifications --> CalendarInvite[Calendar Invitation]
    CalendarInvite --> End([Ready for Appointment])
    
    %% Styling
    classDef access fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef service fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef calendar fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef payment fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef confirm fill:#e0f2f1,stroke:#00796b,stroke-width:2px
    
    class Homepage,EmailLink,SMSLink access
    class AltMedFollow,GAPSFollow,WeightFollow,CounselFollow,EquineFollow service
    class AltMedCal,GAPSCal,WeightCal,CounselCal,EquineCal,AllView,FilterView calendar
    class Payment,Card,Saved,Alternative payment
    class Confirmed,Notifications,CalendarInvite confirm
```

## Process Details

### 1. Direct Access Points
- **Website**: "Book Follow-up" button on homepage
- **Email Reminders**: Direct booking links
- **SMS Reminders**: Mobile-friendly booking links
- **No Login Required**: Simple verification process

### 2. Service Selection

Patients first select their service type, which determines available practitioners:

| Service | Available Practitioners | Duration | Price |
|---------|------------------------|----------|--------|
| Alternative Medicine | Dr Dia, Dr. Shivani, Nurse | 10-15 min | $79 AUD |
| GAPS Coaching | Ramona only | 15 min | $79 AUD |
| Weight Loss | Dr Dia, Dr. Shivani, Nurse | TBD | TBD |
| Counseling | Counselor only | TBD | TBD |
| Equine Therapy | Equine Therapist only | TBD | TBD |

### 3. Calendar Features

#### Combined Calendar View
For services with multiple practitioners:
- See all available time slots
- Color-coded by practitioner
- Filter by specific practitioner
- Show next available for each

#### Smart Filtering
- **By Practitioner**: See only preferred provider
- **By Time**: Morning, afternoon, evening slots
- **By Day**: Weekday vs weekend availability
- **By Location**: Telehealth vs in-person (Dr. Shivani)

### 4. Booking Process

#### Information Required
- Patient verification (email/phone)
- Appointment type confirmation
- Brief reason for visit (optional)
- Insurance updates (if applicable)

#### Payment Options
- Credit/debit card
- Saved payment methods
- HSA/FSA cards
- Payment plans (if available)

### 5. Confirmation & Reminders

#### Immediate Confirmation
- Email with appointment details
- SMS confirmation
- Calendar invite (.ics file)
- Pre-appointment instructions

#### Automated Reminders
- 48 hours before: Email reminder
- 24 hours before: SMS reminder
- 2 hours before: Final reminder
- Rescheduling links included

## Key Advantages

### ⚡ Self-Service Efficiency
- No phone calls needed
- 24/7 booking availability
- Instant confirmation
- Easy rescheduling

### 🎯 Smart Practitioner Display
- Only shows relevant providers
- Real-time availability
- Practitioner profiles available
- Previous provider prioritized

### 💰 Streamlined Payment
- Secure online processing
- Multiple payment options
- Automatic receipts
- Insurance integration ready

### 📱 Mobile Optimized
- Responsive design
- One-thumb navigation
- Quick booking flow
- Mobile calendar sync

[← Back to Overview](./patient-booking-overview.md) | [Next: Service Triage Process →](./service-triage-process.md)