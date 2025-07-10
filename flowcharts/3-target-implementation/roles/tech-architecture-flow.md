# Technical Architecture Flow - Future Options

## Overview
This flowchart shows the enhanced technical architecture supporting 5 services, waitlist management, and scalable infrastructure.

```mermaid
graph TD
    Start([User Request]) --> Gateway[API Gateway]
    
    Gateway --> RateLimit{Rate Limiting}
    RateLimit -->|Exceeded| Throttle[429 Too Many Requests]
    RateLimit -->|Allowed| Router[Service Router]
    
    %% Service Routing
    Router --> ServiceType{Service Type}
    
    ServiceType -->|Booking| BookingService[Booking Microservice]
    ServiceType -->|Patient| PatientService[Patient Microservice]
    ServiceType -->|Payment| PaymentService[Payment Microservice]
    ServiceType -->|Waitlist| WaitlistService[Waitlist Microservice]
    ServiceType -->|Analytics| AnalyticsService[Analytics Microservice]
    
    %% Booking Service Details
    BookingService --> BookingLogic{Booking Logic}
    
    BookingLogic --> ServiceConfig[Service Configuration:<br/>• Alt Medicine: 15-20min<br/>• GAPS: 60/15min<br/>• Weight Loss: TBD<br/>• Counseling: TBD<br/>• Equine: TBD]
    
    ServiceConfig --> CalendarIntegration[Multi-Calendar Integration]
    CalendarIntegration --> CalendarTypes{Calendar Systems}
    
    CalendarTypes --> CalendlyAPI[Calendly API:<br/>• 8+ Calendars<br/>• Service-specific<br/>• Duration rules<br/>• Availability sync]
    CalendarTypes --> O365Backup[Office 365 Backup:<br/>• Failover support<br/>• Cross-sync<br/>• Availability check]
    
    %% Waitlist Service
    WaitlistService --> WaitlistOps{Waitlist Operations}
    
    WaitlistOps --> AddWaitlist[Add to Waitlist]
    WaitlistOps --> CheckPosition[Check Position]
    WaitlistOps --> NotifyLaunch[Notify on Launch]
    WaitlistOps --> ConvertActive[Convert to Active]
    
    AddWaitlist --> WaitlistDB[(Waitlist Database:<br/>• Service interest<br/>• Contact prefs<br/>• Priority score<br/>• Timestamp)]
    
    %% Patient Service
    PatientService --> PatientOps{Patient Operations}
    
    PatientOps --> CreatePatient[Create Profile]
    PatientOps --> UpdatePatient[Update Info]
    PatientOps --> ServiceHistory[Service History]
    PatientOps --> MultiService[Multi-Service Records]
    
    ServiceHistory --> PatientDB[(Patient Database:<br/>• Demographics<br/>• Service enrollments<br/>• Visit history<br/>• Referrals)]
    
    %% Payment Service
    PaymentService --> PaymentOps{Payment Operations}
    
    PaymentOps --> ServicePricing[Dynamic Pricing:<br/>• Service-based<br/>• Package deals<br/>• Promotions]
    PaymentOps --> ProcessPayment[Process Payment]
    PaymentOps --> Refunds[Handle Refunds]
    
    ProcessPayment --> PaymentGateway[Payment Gateway:<br/>• Stripe primary<br/>• PayPal backup<br/>• HSA/FSA support]
    
    %% Analytics Service
    AnalyticsService --> AnalyticsOps{Analytics Operations}
    
    AnalyticsOps --> ServiceMetrics[Service Metrics:<br/>• Utilization<br/>• Revenue<br/>• Growth]
    AnalyticsOps --> WaitlistMetrics[Waitlist Analytics:<br/>• Growth rate<br/>• Conversion potential<br/>• Geographic data]
    AnalyticsOps --> FunnelMetrics[Funnel Performance:<br/>• By service<br/>• By source<br/>• Conversion rates]
    
    ServiceMetrics --> AnalyticsDB[(Analytics Database:<br/>• Time series data<br/>• Aggregations<br/>• Real-time metrics)]
    
    %% Event System
    CalendlyAPI --> EventBus[Event Bus:<br/>• Booking created<br/>• Appointment updated<br/>• Cancellation<br/>• Waitlist activity]
    PaymentGateway --> EventBus
    WaitlistDB --> EventBus
    
    EventBus --> EventHandlers{Event Handlers}
    
    EventHandlers --> NotificationHandler[Notifications:<br/>• Email (SendGrid)<br/>• SMS (Twilio)<br/>• Push notifications]
    EventHandlers --> IntegrationHandler[Integrations:<br/>• MediRecords sync<br/>• Calendar updates<br/>• CRM updates]
    EventHandlers --> AnalyticsHandler[Analytics:<br/>• Event tracking<br/>• Funnel updates<br/>• KPI calculation]
    
    %% Data Flow
    PatientDB --> DataLake[(Data Lake:<br/>• Raw event storage<br/>• Audit trails<br/>• Compliance data<br/>• ML training data)]
    AnalyticsDB --> DataLake
    WaitlistDB --> DataLake
    
    DataLake --> MLPipeline[ML Pipeline:<br/>• Demand prediction<br/>• Churn analysis<br/>• Optimal scheduling<br/>• Waitlist conversion]
    
    %% Infrastructure
    Router --> Cache[Redis Cache:<br/>• Session data<br/>• API responses<br/>• Rate limits<br/>• Feature flags]
    
    BookingService --> ServiceMesh[Service Mesh:<br/>• Service discovery<br/>• Load balancing<br/>• Circuit breakers<br/>• Distributed tracing]
    PatientService --> ServiceMesh
    PaymentService --> ServiceMesh
    WaitlistService --> ServiceMesh
    AnalyticsService --> ServiceMesh
    
    %% Response Flow
    ServiceMesh --> ResponseBuilder[Response Builder]
    Cache --> ResponseBuilder
    ResponseBuilder --> Gateway
    Gateway --> End([Client Response])
    
    %% Styling
    classDef service fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    classDef data fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef integration fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef infra fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef event fill:#e0f2f1,stroke:#00796b,stroke-width:2px
    
    class BookingService,PatientService,PaymentService,WaitlistService,AnalyticsService service
    class PatientDB,WaitlistDB,AnalyticsDB,DataLake data
    class CalendlyAPI,PaymentGateway,NotificationHandler integration
    class Gateway,ServiceMesh,Cache,MLPipeline infra
    class EventBus,EventHandlers event
```

## Service Configuration Management

### Dynamic Service Configuration
```yaml
services:
  alternative_medicine:
    active: true
    durations:
      initial: [15, 20]  # min
      followup: [10, 15]
    pricing:
      initial: 119
      followup: 79
    calendars: ["doctor1", "doctor2", "nurse"]
    
  gaps_coaching:
    active: true
    durations:
      initial: [60]
      followup: [15]
    pricing:
      initial: 195
      followup: 79
    calendars: ["gaps_coach"]
    
  weight_loss:
    active: true
    durations: 
      initial: [TBD]
      followup: [TBD]
    pricing:
      initial: TBD
      followup: TBD
    calendars: ["doctor1", "doctor2", "nurse"]
    
  counseling:
    active: false
    waitlist: true
    launch_date: TBD
    
  equine_therapy:
    active: false
    waitlist: true
    launch_date: TBD
```

## Microservices Architecture

### Booking Microservice
**Responsibilities:**
- Calendar availability aggregation
- Service-specific booking rules
- Multi-practitioner coordination
- Appointment validation

**Key Features:**
- Real-time availability caching
- Conflict detection
- Service duration enforcement
- Practitioner matching

### Waitlist Microservice
**Responsibilities:**
- Interest capture and scoring
- Launch notification system
- Conversion tracking
- Geographic analysis

**Waitlist Scoring Algorithm:**
```python
priority_score = (
    interest_level * 0.3 +
    wait_time * 0.2 +
    engagement * 0.2 +
    location_match * 0.3
)
```

### Payment Microservice
**Responsibilities:**
- Service-based pricing
- Multi-service packages
- Payment processing
- Refund handling

**Pricing Engine:**
```javascript
calculateTotal(services) {
  let total = 0;
  let discount = 0;
  
  // Base pricing
  services.forEach(service => {
    total += pricing[service.type][service.visitType];
  });
  
  // Multi-service discount
  if (services.length > 1) {
    discount = total * 0.1; // 10% multi-service
  }
  
  return total - discount;
}
```

## Integration Points

### Calendar Integration Layer
```
Calendly API v2
├── Webhook subscriptions
│   ├── invitee.created
│   ├── invitee.canceled
│   └── availability.changed
├── Availability API
│   ├── GET /user_availability
│   ├── GET /event_types
│   └── POST /scheduling_links
└── Event Types
    ├── Service-specific configs
    ├── Duration rules
    └── Buffer times
```

### Event-Driven Architecture

**Event Flow:**
1. **Booking Created**
   - Update availability cache
   - Send confirmations
   - Update analytics
   - Sync to MediRecords

2. **Waitlist Entry**
   - Calculate priority score
   - Segment for marketing
   - Schedule nurture campaign
   - Track interest metrics

3. **Service Launch**
   - Notify waitlist
   - Open booking slots
   - Track conversion
   - Update configurations

## Scalability Considerations

### Horizontal Scaling
- Kubernetes orchestration
- Auto-scaling policies
- Service mesh for load distribution
- Database read replicas

### Performance Optimization
- Redis caching layer
- CDN for static assets
- API response caching
- Query optimization

### Monitoring & Observability
```
Monitoring Stack:
├── Prometheus (metrics)
├── Grafana (visualization)
├── ELK Stack (logs)
├── Jaeger (tracing)
└── PagerDuty (alerts)
```

## Security & Compliance

### HIPAA Compliance
- Encryption at rest and in transit
- Audit logging
- Access controls
- Data retention policies

### API Security
- OAuth 2.0 authentication
- API key management
- Rate limiting per client
- Request validation

## Deployment Pipeline

### CI/CD Flow
```yaml
pipeline:
  - stage: test
    - unit_tests
    - integration_tests
    - security_scan
    
  - stage: build
    - docker_build
    - push_to_registry
    
  - stage: deploy_staging
    - deploy_to_k8s
    - run_e2e_tests
    - performance_tests
    
  - stage: deploy_production
    - blue_green_deployment
    - health_checks
    - rollback_if_failed
```

## Future Enhancements

### Phase 1: Current Implementation
- 3 active services
- Basic waitlist capture
- Manual processes

### Phase 2: Full Automation
- All 5 services active
- Automated waitlist conversion
- ML-driven scheduling

### Phase 3: Advanced Features
- Predictive analytics
- Dynamic pricing
- Automated care paths
- AI-powered triage

[← Back to Doctor Flow](./doctor-appointment-flow.md) | [Back to Overview →](./README.md)