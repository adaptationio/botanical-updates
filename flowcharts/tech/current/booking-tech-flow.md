# Current Booking System Technical Architecture

## Overview
This flowchart represents the current technical architecture for the Botaniqal booking system using Calendly, Office 365, AWS, and MediRecords integration.

```mermaid
flowchart TD
    %% User Interface Layer
    User[User Browser] --> Website[Botaniqal Website]
    Website --> BookingChoice{Booking Type}
    
    BookingChoice -->|Initial $89| CalendlyInitial[Calendly Initial Consult]
    BookingChoice -->|Follow-up $69| CalendlyFollowup[Calendly Follow-up]
    
    %% Calendly Layer
    CalendlyInitial --> StripePayment1[Stripe Payment Gateway]
    CalendlyFollowup --> StripePayment2[Stripe Payment Gateway]
    
    StripePayment1 --> CalendarCreate1[Create Calendar Event]
    StripePayment2 --> CalendarCreate2[Create Calendar Event]
    
    CalendarCreate1 --> Office365[Dr's Office 365 Calendar]
    CalendarCreate2 --> Office365
    
    %% Webhook System
    Office365 --> MSGraph[MS Graph API Webhook]
    MSGraph -->|Event Created| WebhookRenewal[Webhook Auto-Renewal]
    WebhookRenewal -->|Every few days| MSGraph
    
    MSGraph --> AWSGateway1[AWS API Gateway - Booking Endpoint]
    
    %% AWS Processing Layer
    AWSGateway1 --> Lambda1[Lambda Function - Booking Processor]
    
    Lambda1 --> ExtractData[Extract Calendar Data]
    ExtractData --> FormatData[Reformat for MediRecords]
    
    FormatData --> PythonLib[Custom Python Library]
    PythonLib --> MediAPI[MediRecords API]
    
    %% MediRecords Processing
    MediAPI --> CheckUser{User Exists?}
    CheckUser -->|No| CreateUser[Create New User]
    CheckUser -->|Yes| UpdateUser[Update User]
    
    CreateUser --> AddPlaceholders[Add Placeholder Data]
    AddPlaceholders --> CreateBooking[Create MediRecords Booking]
    UpdateUser --> CreateBooking
    
    CreateBooking --> MediCalendar[MediRecords Calendar]
    
    %% Initial Consult Additional Flow
    CalendarCreate1 --> IntakeForm[Intake Form Page]
    IntakeForm --> FillDetails[Patient Details Form]
    FillDetails --> HealthQuestions[Health Questions]
    HealthQuestions --> ConsentForm[Digital Consent Form]
    ConsentForm --> SubmitForm[Submit Form]
    
    %% Intake Form Processing
    SubmitForm --> FormWebhook[Form Submission Webhook]
    FormWebhook --> AWSGateway2[AWS API Gateway - Intake Endpoint]
    AWSGateway2 --> Lambda2[Lambda Function - Intake Processor]
    
    Lambda2 --> ParseForm[Parse Form Data]
    ParseForm --> MapFields[Map to MediRecords Fields]
    MapFields --> PythonLib2[Custom Python Library]
    PythonLib2 --> MediAPI2[MediRecords API]
    
    MediAPI2 --> UpdatePatient[Update Patient Details]
    UpdatePatient --> ReplacePlaceholders[Replace Placeholder Data]
    
    %% Notifications Layer
    StripePayment1 --> StripeEmail1[Payment Confirmation Email]
    StripePayment2 --> StripeEmail2[Payment Confirmation Email]
    
    StripeEmail1 --> PatientEmail1[Patient Email]
    StripeEmail1 --> EnquiriesEmail1[Enquiries Email]
    StripeEmail2 --> PatientEmail2[Patient Email]
    StripeEmail2 --> EnquiriesEmail2[Enquiries Email]
    
    CreateBooking --> MediSMS[MediRecords SMS Service]
    MediSMS --> PatientSMS[Patient SMS Confirmation]
    
    CreateBooking --> CalendarInvite[Calendar Invite Email]
    CalendarInvite --> PatientCalendar[Patient Calendar]
    
    SubmitForm --> FormEmails[Form Submission Emails]
    FormEmails --> PatientCopy[Patient Form Copy]
    FormEmails --> EnquiriesCopy[Enquiries Form Copy]
    
    %% Data Stores
    MediCalendar -.-> MediDB[(MediRecords Database)]
    Office365 -.-> O365DB[(Office 365 Storage)]
    Lambda1 -.-> CloudWatch1[CloudWatch Logs]
    Lambda2 -.-> CloudWatch2[CloudWatch Logs]
    
    %% Styling
    classDef user fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef calendly fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef aws fill:#fff8e1,stroke:#f57c00,stroke-width:3px
    classDef medirecords fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef notification fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef webhook fill:#f3e5f5,stroke:#6a1b9a,stroke-width:3px
    classDef form fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    classDef data fill:#f5f5f5,stroke:#616161,stroke-width:2px
    
    class User,Website,BookingChoice user
    class CalendlyInitial,CalendlyFollowup,StripePayment1,StripePayment2 calendly
    class AWSGateway1,AWSGateway2,Lambda1,Lambda2,CloudWatch1,CloudWatch2 aws
    class MediAPI,MediAPI2,CheckUser,CreateUser,UpdateUser,CreateBooking,MediCalendar,UpdatePatient medirecords
    class StripeEmail1,StripeEmail2,PatientEmail1,PatientEmail2,EnquiriesEmail1,EnquiriesEmail2,MediSMS,PatientSMS,CalendarInvite,FormEmails notification
    class MSGraph,WebhookRenewal,FormWebhook webhook
    class IntakeForm,FillDetails,HealthQuestions,ConsentForm,SubmitForm form
    class MediDB,O365DB,CloudWatch1,CloudWatch2 data
```

## Technical Components

### Frontend Layer
- **Website**: Botaniqal website (www.botaniqal.com.au)
- **Calendly Widgets**: Embedded for Initial ($89) and Follow-up ($69) bookings
- **Intake Form**: Custom form for patient details post-booking

### Calendar Integration
- **Calendly**: Two calendar types (Initial Consult, Follow-up)
- **Office 365**: Doctor's calendar synchronized with Calendly
- **MS Graph API**: Webhook for calendar events with auto-renewal

### Payment Processing
- **Stripe Integration**: Embedded in Calendly booking flow
- **Payment Confirmation**: Automated emails to patient and enquiries

### AWS Infrastructure
- **API Gateway**: Two endpoints (booking, intake form)
- **Lambda Functions**: 
  - Booking processor for calendar events
  - Intake processor for form submissions
- **CloudWatch**: Logging and monitoring

### MediRecords Integration
- **Custom Python Library**: Handles API communication
- **Patient Management**: 
  - Checks existing patients by email
  - Creates new patients with placeholders
  - Updates patient details from intake form
- **Booking Creation**: Syncs with MediRecords calendar

### Data Flow Specifics

#### Initial Consultation Flow
1. User selects Initial Consult ($89) on website
2. Calendly booking with Stripe payment
3. Office 365 calendar event created
4. MS Graph webhook triggers AWS Lambda
5. Lambda processes booking data
6. MediRecords patient/booking created
7. User redirected to intake form
8. Form submission triggers second Lambda
9. Patient details updated in MediRecords
10. Notifications sent (SMS, email, calendar)

#### Follow-up Consultation Flow
1. User selects Follow-up ($69) on website
2. Calendly booking with Stripe payment
3. Office 365 calendar event created
4. MS Graph webhook triggers AWS Lambda
5. Lambda processes booking data
6. MediRecords booking created for existing patient
7. Notifications sent (SMS, email, calendar)

### Key Integration Points
- **Calendly ↔ Office 365**: Direct calendar sync
- **Office 365 → AWS**: MS Graph webhooks
- **AWS → MediRecords**: Custom Python library
- **Stripe → Email**: Payment confirmations
- **MediRecords → SMS**: Booking confirmations

### Critical Requirements
- **Calendar Sync**: MediRecords availability must match Calendly
- **Webhook Renewal**: MS Graph webhooks renewed every few days
- **Data Mapping**: Form fields mapped to MediRecords schema
- **Email Notifications**: Both patient and enquiries receive copies

### Security & Compliance
- **HTTPS**: All API communications
- **AWS IAM**: Lambda function permissions
- **API Keys**: Secure storage for MediRecords API
- **PCI Compliance**: Through Stripe integration
- **HIPAA Considerations**: Patient data handling