# Current System Flowcharts

This folder contains flowcharts documenting the **CURRENT** Botaniqal booking system as it exists today.

## System Overview
- **Single Practitioner**: Dr. Dia only
- **Direct Booking**: Patients book and pay upfront ($89 initial, $69 follow-up)
- **Two-Step Process**: Book first, then fill intake form

## Contents

### 📋 Admin Flows
- `admin/admin-flow.md` - Basic admin operations and reporting
- `admin/booking-admin-flow.md` - Manual booking management and calendar sync

### 👨‍⚕️ Doctor Workflow  
- `doctor/appointment-flow.md` - Dr. Dia's consultation process

### 💻 Technical Architecture
- `tech/tech-flow.md` - Current monolithic architecture
- `tech/booking-tech-flow.md` - Calendly + MediRecords integration

### 📅 Booking Process
- `booking/comprehensive-booking-flow.md` - End-to-end booking system
- `booking/patient-booking-flow.md` - Patient's booking journey

## Key Characteristics
- Single Calendly calendar
- Automated Stripe payments
- JotForm intake after booking
- Manual processes for many admin tasks

---

💡 **Looking for the new system?** See [2-new-update](../2-new-update/)