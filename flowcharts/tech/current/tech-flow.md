# Current Technical Architecture Flow

## Overview
This flowchart represents the current technical architecture and data flow.

```mermaid
flowchart TD
    %% Client Layer
    Browser[Web Browser] --> DNS[DNS Lookup]
    DNS --> LB[Load Balancer]
    
    %% Web Server Layer
    LB --> Web1[Web Server 1]
    LB --> Web2[Web Server 2]
    
    Web1 --> App[Application Server]
    Web2 --> App
    
    %% Application Layer
    App --> Router{Request Router}
    
    Router --> Static[Static Assets]
    Router --> Dynamic[Dynamic Content]
    
    Static --> CDN[CDN]
    CDN --> Browser
    
    Dynamic --> Auth{Authentication}
    Auth -->|Valid| Session[Session Management]
    Auth -->|Invalid| Login[Login Page]
    
    Session --> Business{Business Logic}
    
    %% Business Logic
    Business --> UserOps[User Operations]
    Business --> ProductOps[Product Operations]
    Business --> OrderOps[Order Operations]
    Business --> AdminOps[Admin Operations]
    
    %% Data Layer
    UserOps --> DB[(MySQL Database)]
    ProductOps --> DB
    OrderOps --> DB
    AdminOps --> DB
    
    %% External Services
    OrderOps --> Payment[Payment Gateway]
    OrderOps --> Email[Email Service]
    ProductOps --> ImageStore[Image Storage]
    
    Payment --> PaymentAPI[Stripe API]
    Email --> SMTP[SMTP Server]
    ImageStore --> S3[AWS S3]
    
    %% Response Flow
    DB --> Cache[Redis Cache]
    Cache --> App
    PaymentAPI --> App
    SMTP --> App
    S3 --> CDN
    
    %% Monitoring
    App --> Logs[Application Logs]
    Logs --> Monitor[Basic Monitoring]
    
    %% Backup
    DB --> Backup[Nightly Backup]
    Backup --> BackupStorage[Backup Storage]
    
    %% Styling
    classDef client fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef server fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef data fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef external fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    classDef monitor fill:#ffebee,stroke:#c62828,stroke-width:2px
    
    class Browser,CDN client
    class LB,Web1,Web2,App,Router server
    class DB,Cache,BackupStorage data
    class Payment,Email,ImageStore,PaymentAPI,SMTP,S3 external
    class Logs,Monitor,Backup monitor
```

## Current Stack
- **Frontend**: HTML, CSS, JavaScript (jQuery)
- **Backend**: PHP 7.4
- **Database**: MySQL 5.7
- **Cache**: Redis
- **Web Server**: Apache
- **CDN**: CloudFront
- **Storage**: AWS S3

## Booking System Stack
- **Booking Platform**: Calendly (Initial & Follow-up calendars)
- **Calendar Sync**: Office 365 with MS Graph API webhooks
- **Payment**: Stripe integrated with Calendly
- **Serverless**: AWS API Gateway + Lambda functions
- **EHR Integration**: MediRecords API with custom Python library
- **Forms**: Custom intake form post-booking
- **Notifications**: Email (Stripe/Form), SMS (MediRecords)

## Architecture Details
- Traditional monolithic architecture
- Session-based authentication
- Synchronous request processing
- Manual scaling required
- Basic monitoring setup
- Hybrid cloud approach (on-premises + AWS Lambda)

## Booking System Architecture
- Event-driven architecture via webhooks
- Serverless functions for booking processing
- Multiple API integrations (Calendly, Stripe, MediRecords)
- Asynchronous processing for form submissions
- See [Detailed Booking Technical Flow](./booking-tech-flow.md)

## Limitations
- No microservices
- Limited API endpoints
- No real-time features
- Manual deployment process
- Basic error handling
- No container orchestration
- Complex multi-system dependencies
- Manual webhook renewal required