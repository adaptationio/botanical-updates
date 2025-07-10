# New Update System Flowcharts

This folder contains flowcharts for the **NEW** multi-practitioner booking system being implemented.

## Major Changes
- **FREE Triage Consultation**: 20-minute assessment before booking
- **Multi-Practitioner**: 6 healthcare providers
- **Phone Booking**: Consultant-assisted booking with payment
- **New Pricing**: $119 initial, $79 follow-up (was $89/$69)

## Contents

### 📋 Admin Flows
- `admin/admin-flow.md` - Enhanced admin with multi-calendar management
- `admin/booking-admin-flow.md` - Phone booking support and reporting

### 👨‍⚕️ Doctor Workflow
- `doctor/appointment-flow.md` - Multi-practitioner collaboration

### 🏥 Consultant Workflow (NEW!)
- `consultant/triage-flow.md` - Free consultation and triage process

### 💻 Technical Architecture  
- `tech/tech-flow.md` - Microservices architecture
- `tech/booking-tech-flow.md` - Multi-calendar webhook orchestration

### 📅 Booking Process
- `booking/comprehensive-booking-flow.md` - New 5-stakeholder system
- `booking/patient-booking-flow.md` - Triage-first patient journey

## New Features
- Free initial consultation with dynamic intake forms
- Consultant uses specialty forms during call (not sent to patient)
- Different forms based on service interest (weight loss, GAPS, alt medicine)
- Practitioner matching based on needs
- Phone payment collection
- Multiple service types (Alternative Medicine, GAPS, etc.)
- In-person Melbourne appointments

---

🔄 **Compare with current:** [1-current-system](../1-current-system/)
🎯 **Target implementation:** [3-target-implementation](../3-target-implementation/)