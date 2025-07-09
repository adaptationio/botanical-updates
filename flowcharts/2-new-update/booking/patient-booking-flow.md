# New Update Patient Booking Flow

## Overview
This flowchart represents the updated booking flow ready for deployment with free initial consultations, consultant triage, and multiple practitioners.

```mermaid
flowchart TD
    Start([User visits www.botaniqal.com.au]) --> Welcome[Enhanced Landing Page]
    
    Welcome --> Choice{Select Consultation Type}
    
    %% Initial Consultation Path - FREE
    Choice -->|Initial Consult FREE| InitialInfo[Free Consultation Info]
    InitialInfo --> StartBooking[Start Booking]
    
    StartBooking --> CalendlyConsultant[Consultant Calendar - Calendly]
    CalendlyConsultant --> SelectTime[Select Available Time]
    SelectTime --> BasicDetails[Enter Basic Details]
    BasicDetails --> NoPayment[✓ No Payment Required]
    
    NoPayment --> BookingConfirmed[Booking Confirmed]
    BookingConfirmed --> MiniIntake[Mini Intake + Consent Form]
    
    MiniIntake --> BasicHealth[Basic Health Info]
    BasicHealth --> ConsentSign[Digital Consent Signature]
    ConsentSign --> SubmitIntake[Submit Form with Consent]
    
    SubmitIntake --> Notifications1[Receive Confirmations]
    Notifications1 --> WaitAppt[Wait for Appointment]
    
    %% Consultant Appointment
    WaitAppt --> ConsultantCall[Consultant/Ramona Calls Patient<br/>(Ramona (GAPS Coach) currently<br/>handles free consults)]
    ConsultantCall --> Triage[Triage Assessment]
    
    Triage --> DynamicForm[Dynamic Question Form]
    DynamicForm --> DetermineNext{Determine Best Next Step}
    
    DetermineNext -->|Doctor Needed| RecDoctor[Recommend Doctor]
    DetermineNext -->|Nurse Practitioner| RecNurse[Recommend Nurse Practitioner]
    DetermineNext -->|GAPS Coach| RecGAPS[Recommend Ramona (GAPS Coach)]
    DetermineNext -->|Not Eligible| RecAlternative[Alternative Options]
    
    RecDoctor --> PhoneBooking[Book & Pay Over Phone]
    RecNurse --> PhoneBooking
    RecGAPS --> PhoneBooking
    RecAlternative --> ProvideResources[Provide Resources]
    
    PhoneBooking --> SelectPractitioner{Select Specific Practitioner}
    SelectPractitioner -->|Dr Dia| BookDoc1[Select Time with Dr Dia]
    SelectPractitioner -->|Dr. Shivani| BookDoc2[Select Time with Dr. Shivani]
    SelectPractitioner -->|Nurse Practitioner| BookNurse[Select Time with Nurse]
    SelectPractitioner -->|Ramona| BookGAPS[Select Time with Ramona]
    
    BookDoc1 --> ConfirmTimePhone[Confirm Time Over Phone]
    BookDoc2 --> ConfirmTimePhone
    BookNurse --> ConfirmTimePhone
    BookGAPS --> ConfirmTimePhone
    
    ConfirmTimePhone --> ProcessPayment[Process Payment Over Phone]
    ProcessPayment --> SecondBookingConfirmed[Practitioner Booking Confirmed]
    SecondBookingConfirmed --> FullIntake[Full Intake Form Sent]
    FullIntake --> CompleteIntake[Complete Full Intake]
    CompleteIntake --> ReadyForAppt[Ready for Practitioner Appointment]
    
    %% Follow-up Path
    Choice -->|Follow-up Consult| FollowUpInfo[Follow-up Options]
    FollowUpInfo --> CombinedCalendar[Combined Availability Calendar<br/>Shows All Practitioners<br/>For Your Service Type]
    
    CombinedCalendar --> ViewOptions{View Options}
    ViewOptions -->|All Available| ShowAll[Show All Practitioners]
    ViewOptions -->|Filter| FilterTabs[Select Practitioner Tab]
    
    FilterTabs --> TabChoice{Select Tab}
    TabChoice -->|Dr Dia| ShowDoc1[Dr Dia Calendar]
    TabChoice -->|Dr. Shivani| ShowDoc2[Dr. Shivani Calendar]
    TabChoice -->|Nurse| ShowNurse[Nurse Calendar]
    TabChoice -->|Ramona| ShowGAPS[Ramona Calendar]
    TabChoice -->|All| ShowAll
    
    ShowAll --> SelectFollowTime[Select Time & Practitioner]
    ShowDoc1 --> SelectFollowTime
    ShowDoc2 --> SelectFollowTime
    ShowNurse --> SelectFollowTime
    ShowGAPS --> SelectFollowTime
    
    SelectFollowTime --> EnterDetails[Enter Details]
    EnterDetails --> PayFollowUp[Pay Follow-up Fee]
    PayFollowUp --> FollowUpConfirmed[Follow-up Confirmed]
    
    FollowUpConfirmed --> Notifications2[Receive Confirmations]
    Notifications2 --> End([Ready for Appointment])
    
    ProvideResources --> End
    ReadyForAppt --> End
    
    %% Styling
    classDef free fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px
    classDef consultant fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    classDef payment fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef choice fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef calendar fill:#fff8e1,stroke:#f57c00,stroke-width:2px
    classDef form fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    
    class InitialInfo,NoPayment,ConsultantCall free
    class Triage,DynamicForm,DetermineNext,RecDoctor,RecNurse,RecGAPS consultant
    class ProcessPayment,PayFollowUp payment
    class Choice,ViewOptions,FilterTabs,TabChoice,SelectPractitioner choice
    class CalendlyConsultant,CombinedCalendar,ShowAll,ShowDoc1,ShowDoc2,ShowNurse,ShowGAPS calendar
    class MiniIntake,BasicHealth,ConsentSign,DynamicForm,FullIntake form
```

## Key Changes in New Update

### 1. Free Initial Consultation
- **No Payment Required**: Initial consults are completely free (20 minutes)
- **Consultant-Led**: Professional consultant handles initial assessment
- **Mini Intake + Consent**: Combined smaller intake form with consent signature
- **Triage Focus**: Determines best practitioner match
- **Phone Booking & Payment**: Consultant books next appointment and takes payment over phone

### 2. Multiple Practitioners
- **Dr Dia**: Telehealth alternative medicine
- **Doctor 2 (Dr. Shivani)**: Telehealth & In-person consultations (Melbourne clinic)
- **1 Nurse Practitioner**: Telehealth alternative medicine
- **Ramona (GAPS Coach)**: Specialized nutrition coaching (currently also performs free consultations)
- **Individual Calendars**: Each practitioner has dedicated Calendly

**Service Assignments:**
- Alternative Medicine: Dr Dia, Dr. Shivani, Nurse Practitioner
- Weight Loss: Dr Dia, Dr. Shivani, Nurse Practitioner
- GAPS Coaching: Ramona only

### 3. Smart Triage Process
- **Dynamic Forms**: Consultant uses dynamic question forms
- **Guided Assessment**: Questions help determine best practitioner
- **Phone Booking**: Personalized booking over phone after triage
- **Eligibility Check**: Ensures patients get appropriate care

### 4. Enhanced Follow-up Booking
- **Combined Calendar**: View availability of all practitioners who can service your appointment type
- **Service-Based Display**: Only shows relevant practitioners (e.g., Alt Med shows Doctors + Nurse, GAPS shows only GAPS Coach)
- **Dynamic Filtering**: Tab-based filtering by practitioner
- **Direct Booking**: Self-service for returning patients
- **Practitioner Choice**: Select preferred provider from available options

### 5. Improved User Experience
- **Clear Path Separation**: Initial vs Follow-up clearly defined
- **No Upfront Payment**: Reduces barrier for initial consultation
- **Personal Touch**: Phone-based booking after assessment
- **Flexibility**: Multiple practitioner options

## Technical Implementation
- Same core tech stack (Webflow, Calendly, AWS, MediRecords)
- Additional Calendly calendars for each practitioner
- Dynamic form integration for consultant questions
- Tab-based JavaScript for calendar filtering
- Phone payment processing integration