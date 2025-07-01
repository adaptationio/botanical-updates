# Current User Experience Flow

## Overview
This flowchart represents the current user experience flow for the Botanical website.

```mermaid
flowchart TD
    Start([User Visits Site]) --> HomePage[Landing Page]
    HomePage --> Browse{Browse Options}
    
    Browse --> Plants[Browse Plants]
    Browse --> Categories[Browse Categories]
    Browse --> Search[Search Products]
    
    Plants --> PlantDetail[Plant Details Page]
    Categories --> CategoryList[Category Listing]
    Search --> SearchResults[Search Results]
    
    CategoryList --> PlantDetail
    SearchResults --> PlantDetail
    
    PlantDetail --> AddCart{Add to Cart?}
    AddCart -->|Yes| Cart[Shopping Cart]
    AddCart -->|No| Continue[Continue Browsing]
    
    Continue --> Browse
    
    Cart --> Checkout{Checkout?}
    Checkout -->|Yes| Login{User Logged In?}
    Checkout -->|No| Continue
    
    Login -->|No| Register[Registration/Login]
    Login -->|Yes| ShippingInfo[Shipping Information]
    
    Register --> ShippingInfo
    ShippingInfo --> Payment[Payment Processing]
    Payment --> Confirmation[Order Confirmation]
    Confirmation --> End([End])
    
    %% Styling
    classDef startEnd fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef process fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef decision fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    
    class Start,End startEnd
    class HomePage,Plants,Categories,Search,PlantDetail,CategoryList,SearchResults,Cart,Register,ShippingInfo,Payment,Confirmation process
    class Browse,AddCart,Checkout,Login decision
```

## Key Features
- Simple browsing experience
- Basic search functionality
- Standard e-commerce checkout flow
- Account creation during checkout

## Pain Points
- No guest checkout option
- Limited filtering options
- No wishlist functionality
- No order tracking