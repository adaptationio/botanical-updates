# New Patient Journey - Detailed Flow

## Overview
This diagram shows the complete journey for first-time patients from landing page to first appointment.

```mermaid
graph TD
    Start([New Patient]) --> Entry{Entry Point}
    
    %% Service Landing Pages
    Entry -->|Direct| ServiceLanding[Service Landing Page]
    Entry -->|Search| SEO[Google/SEO]
    Entry -->|Referral| Referral[Professional Referral]
    
    ServiceLanding --> ServiceType{Which Service?}
    SEO --> ServiceType
    Referral --> ServiceType
    
    %% Service Selection
    ServiceType --> AltMed[Alternative Medicine]
    ServiceType --> GAPS[GAPS Coaching]
    ServiceType --> Weight[Weight Loss]
    ServiceType --> Counsel[Counseling]
    ServiceType --> Equine[Equine Therapy]
    
    %% All converge to free consultation
    AltMed --> FreeCTA[Book FREE Consultation]
    GAPS --> FreeCTA
    Weight --> FreeCTA
    Counsel --> FreeCTA
    Equine --> FreeCTA
    
    %% Booking Process
    FreeCTA --> Calendar[Consultant Calendar]
    Calendar --> SelectTime[Select Time Slot]
    SelectTime --> ContactInfo[Enter Contact Details]
    ContactInfo --> Confirm[Booking Confirmed]
    
    %% Consultation Process
    Confirm --> Wait[Wait for Appointment]
    Wait --> ConsultCall[20-min Phone Consultation]
    
    %% Triage Process
    ConsultCall --> Assessment{Assessment Type}
    
    Assessment -->|Single Service| SingleTriage[Focused Assessment]
    Assessment -->|Multiple Needs| MultiTriage[Comprehensive Assessment]
    Assessment -->|Complex Case| ComplexTriage[Extended Assessment]
    
    %% Form Selection
    SingleTriage --> FormSelect[Select Intake Form]
    MultiTriage --> FormSelect
    ComplexTriage --> FormSelect
    
    FormSelect --> FormType{Form Type}
    FormType --> WeightForm[Weight Loss Form]
    FormType --> MedicalForm[Medical History Form]
    FormType --> GAPSForm[Nutrition Assessment]
    FormType --> MentalForm[Mental Health Form]
    FormType --> MultiForm[Multi-Service Form]
    
    %% Complete Assessment
    WeightForm --> CompleteAssess[Complete During Call]
    MedicalForm --> CompleteAssess
    GAPSForm --> CompleteAssess
    MentalForm --> CompleteAssess
    MultiForm --> CompleteAssess
    
    %% Next Steps
    CompleteAssess --> Recommend[Service Recommendation]
    Recommend --> PractMatch[Practitioner Matching]
    PractMatch --> PhoneBook[Phone Booking Process]
    PhoneBook --> End([Continue to Booking])
    
    %% Styling
    classDef entry fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef service fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef free fill:#c8e6c9,stroke:#1b5e20,stroke-width:3px
    classDef assess fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef form fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    
    class ServiceLanding,SEO,Referral entry
    class AltMed,GAPS,Weight,Counsel,Equine service
    class FreeCTA,ConsultCall free
    class Assessment,SingleTriage,MultiTriage,ComplexTriage assess
    class WeightForm,MedicalForm,GAPSForm,MentalForm,MultiForm form
```

## Process Details

### 1. Entry Points
- **Service Landing Pages**: Targeted marketing funnels
- **SEO/Search**: Organic search results
- **Professional Referrals**: GP or specialist referrals

### 2. Free Consultation Booking
- No payment required
- 20-minute phone consultation
- Basic contact information only
- Immediate confirmation

### 3. Consultation Process

#### Who Performs Consultations?
- **Currently**: Ramona (GAPS Coach) handles all free consultations
- **Future**: May transition to dedicated consultant role

#### Dynamic Form Selection
The consultant selects appropriate intake forms based on:
- Patient's stated interests
- Health concerns discussed
- Service preferences
- Complexity of needs

#### Form Types Available
1. **Weight Loss Form**: BMI, diet history, goals
2. **Medical History Form**: General health assessment
3. **GAPS Nutrition Form**: Gut health, dietary restrictions
4. **Mental Health Form**: Mood, stress, support needs
5. **Multi-Service Form**: Comprehensive for multiple concerns

### 4. Assessment Outcomes

#### Service Recommendations
Based on assessment, consultant recommends:
- Single service focus
- Multi-service integrated care
- Specialty service priority
- Alternative resources if not suitable

#### Practitioner Matching
- **Alternative Medicine**: Dr Dia, Dr. Shivani, or Nurse
- **GAPS**: Ramona only
- **Weight Loss**: Dr Dia, Dr. Shivani, or Nurse
- **Counseling**: Counselor only
- **Equine Therapy**: Equine Therapist only

### 5. Transition to Booking
After assessment, consultant:
1. Explains recommended service(s)
2. Discusses practitioner options
3. Checks availability while on phone
4. Books appointment(s)
5. Processes payment
6. Sends confirmation

## Key Features

### 🎯 Smart Triage
- Dynamic form selection
- Service-specific assessments
- Complexity evaluation
- Appropriate practitioner matching

### 📞 Personal Touch
- Phone-based consultation
- Real-time assessment
- Immediate recommendations
- Assisted booking process

### 🔄 Flexible Pathways
- Single or multi-service options
- Integrated care planning
- Priority-based scheduling
- Alternative recommendations

[← Back to Overview](./patient-booking-overview.md) | [Next: Phone Booking Process →](./phone-booking-process.md)