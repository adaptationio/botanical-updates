# Improved Botaniqal Booking Flow

## Overview
This flowchart represents an improved booking flow that addresses current pain points and enhances user experience.

```mermaid
flowchart TD
    Start([User visits www.botaniqal.com.au]) --> Welcome[Welcome with Clear CTAs]
    
    Welcome --> Choice{Select Consultation Type}
    
    %% Initial Consultation Path
    Choice -->|Initial Consult $89| InitialInfo[Show Initial Consult Info]
    InitialInfo --> StartBooking1[Start Booking]
    
    %% Follow-up Path
    Choice -->|Follow-up Consult| FollowInfo[Show Follow-up Info & Price]
    FollowInfo --> StartBooking2[Start Booking]
    
    %% Unified Booking Flow
    StartBooking1 --> UnifiedForm[Unified Booking + Intake Form]
    StartBooking2 --> UnifiedForm
    
    UnifiedForm --> BasicDetails[Step 1: Basic Details]
    BasicDetails --> SaveProgress1{Save & Continue?}
    SaveProgress1 -->|Continue| Eligibility[Step 2: Eligibility Check]
    SaveProgress1 -->|Save for Later| EmailLink[Email Progress Link]
    
    Eligibility --> MentalHealth{Mental Health Screening}
    MentalHealth -->|Needs Support| AlternativeOptions[Show Alternative Options]
    MentalHealth -->|Eligible| MedicalHistory[Step 3: Medical History]
    
    AlternativeOptions --> TeleHealth{Telehealth Option?}
    TeleHealth -->|Yes| MedicalHistory
    TeleHealth -->|No| ContactClinic[Contact Clinic Details + Resources]
    ContactClinic --> End1([End])
    
    MedicalHistory --> SaveProgress2{Save & Continue?}
    SaveProgress2 -->|Continue| CalendarIntegrated[Step 4: Select Appointment]
    SaveProgress2 -->|Save for Later| EmailLink
    
    CalendarIntegrated --> ProviderSelect{Select Provider}
    ProviderSelect --> DrDia[Dr. Dia]
    ProviderSelect --> NextAvailable[Next Available]
    ProviderSelect --> ProviderProfiles[View Provider Profiles]
    
    DrDia --> TimeSlots[Available Time Slots]
    NextAvailable --> TimeSlots
    ProviderProfiles --> TimeSlots
    
    TimeSlots --> ConfirmTime[Confirm Date & Time]
    ConfirmTime --> Consent[Step 5: Review Consent & Terms]
    
    Consent --> Payment[Step 6: Secure Payment]
    Payment --> PaymentOptions{Payment Method}
    PaymentOptions --> Card[Credit/Debit Card]
    PaymentOptions --> PayPal[PayPal]
    PaymentOptions --> Afterpay[Afterpay]
    
    Card --> ProcessPayment[Process Payment]
    PayPal --> ProcessPayment
    Afterpay --> ProcessPayment
    
    ProcessPayment --> Confirmation[Booking Confirmation Page]
    
    %% Post-Booking
    Confirmation --> ImmediateActions{Immediate Actions}
    ImmediateActions --> AddCalendar[Add to Calendar]
    ImmediateActions --> DownloadSummary[Download Booking Summary]
    ImmediateActions --> ShareDetails[Share with Carer/Family]
    
    Confirmation --> AutomatedFlow{Automated Notifications}
    
    AutomatedFlow --> InstantNotify[Instant Notifications]
    InstantNotify --> SMS[SMS Confirmation]
    InstantNotify --> Email[Email with Details]
    InstantNotify --> IntakeConfirm[Intake Form Confirmation]
    
    AutomatedFlow --> ScheduledReminders[Scheduled Reminders]
    ScheduledReminders --> Day7[7 Days Before]
    ScheduledReminders --> Day1[1 Day Before]
    ScheduledReminders --> Hours2[2 Hours Before]
    
    %% Additional Features
    Confirmation --> AccountCreated{Account Created?}
    AccountCreated -->|Yes| Dashboard[Access Patient Dashboard]
    AccountCreated -->|No| CreateAccount[Option to Create Account]
    
    Dashboard --> Features{Dashboard Features}
    Features --> ViewBookings[View All Bookings]
    Features --> Reschedule[Reschedule Options]
    Features --> CancelBooking[Cancel Booking]
    Features --> MessageProvider[Message Provider]
    
    SMS --> End2([End])
    Email --> End2
    Dashboard --> End2
    
    %% Return User Path
    EmailLink --> ReturnUser[Return to Saved Booking]
    ReturnUser --> UnifiedForm
    
    %% Styling
    classDef startEnd fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    classDef process fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef decision fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef payment fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef notification fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef form fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    classDef improvement fill:#e0f2f1,stroke:#00695c,stroke-width:3px
    
    class Start,End1,End2 startEnd
    class Choice,MentalHealth,TeleHealth,SaveProgress1,SaveProgress2,ProviderSelect,PaymentOptions,ImmediateActions,AutomatedFlow,AccountCreated,Features decision
    class Welcome,InitialInfo,FollowInfo,StartBooking1,StartBooking2,CalendarIntegrated,TimeSlots,ConfirmTime process
    class Payment,Card,PayPal,Afterpay,ProcessPayment payment
    class SMS,Email,Day7,Day1,Hours2 notification
    class UnifiedForm,BasicDetails,Eligibility,MedicalHistory,Consent form
    class AlternativeOptions,ProviderProfiles,Dashboard,AddCalendar,DownloadSummary,ShareDetails,Reschedule improvement
```

## Key Improvements

### 1. Unified Booking Experience
- **Single Form**: Combines booking and intake into one streamlined process
- **Progress Saving**: Users can save and return to complete booking
- **Step Indicators**: Clear progress through booking steps

### 2. Enhanced Eligibility Handling
- **Alternative Options**: Telehealth offered for mental health cases
- **Resources**: Provides helpful resources instead of just rejection
- **Soft Screening**: More nuanced approach to eligibility

### 3. Provider Flexibility
- **Multiple Providers**: Option for next available or specific provider
- **Provider Profiles**: View provider information before selection
- **Smart Scheduling**: Shows best available times

### 4. Payment Options
- **Multiple Methods**: Credit/Debit, PayPal, Afterpay
- **Secure Processing**: PCI-compliant payment handling
- **Clear Pricing**: Transparent fee structure

### 5. Better Communication
- **Instant Confirmation**: Immediate SMS and email
- **Smart Reminders**: 7 days, 1 day, and 2 hours before
- **Calendar Integration**: Add to personal calendar
- **Document Access**: Download booking summary

### 6. Patient Dashboard
- **Account Creation**: Optional but encouraged
- **Booking Management**: View, reschedule, cancel
- **Provider Communication**: Secure messaging
- **History Access**: View past appointments

### 7. Accessibility Features
- **Save Progress**: Return later to complete
- **Share Options**: Share with family/carers
- **Clear Information**: Better explanation of services
- **Support Options**: Multiple ways to get help

## Technical Enhancements
- Single-page application for smooth flow
- Real-time availability updates
- Mobile-responsive design
- Secure data handling
- API integration for all services