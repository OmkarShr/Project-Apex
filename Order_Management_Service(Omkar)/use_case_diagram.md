# Order Management Service - Use Case Diagram

## Comprehensive Use Case Diagram with All Module Connections

```mermaid
graph TD
    %% Actors
    Customer[("👤 Customer")]
    WarehouseManager[("👤 Warehouse Manager")]
    SystemAdmin[("👤 System Administrator")]
    Picker[("👤 Picker")]
    Driver[("👤 Driver")]
    CustomerService[("👤 Customer Service Rep")]
    
    %% Order Management Service Use Cases
    CreateOrder["Create Order"]
    GetOrder["Get Order Details"]
    CancelOrder["Cancel Order"]
    TrackOrder["Track Order Status"]
    UpdateOrderStatus["Update Order Status"]
    ReserveInventory["Reserve Inventory"]
    ValidateOrder["Validate Order"]
    ProcessPayment["Process Payment"]
    CalculateTotal["Calculate Order Total"]
    ModifyOrder["Modify Order"]
    
    %% External Service Connections
    CheckFraud["Check for Fraud"]
    RequestFulfillment["Request Fulfillment"]
    NotifyShipped["Notify Shipped"]
    UpdateInventory["Update Inventory"]
    GenerateShippingLabel["Generate Shipping Label"]
    SendNotification["Send Customer Notification"]
    ProcessRefund["Process Refund"]
    AuthenticateUser["Authenticate User"]
    AuthorizeAccess["Authorize Access"]
    GetProductInfo["Get Product Information"]
    OptimizeRoute["Optimize Delivery Route"]
    TrackDelivery["Track Delivery"]
    CapturePOD["Capture Proof of Delivery"]
    GenerateReport["Generate Analytics Report"]
    PredictDemand["Predict Demand"]
    
    %% Include Relationships
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| ValidateOrder
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| CheckFraud
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| ReserveInventory
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| ProcessPayment
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| AuthenticateUser
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| AuthorizeAccess
    CreateOrder -.->|"&lt;&lt;include&gt;&gt;"| GetProductInfo
    
    UpdateOrderStatus -.->|"&lt;&lt;include&gt;&gt;"| AuthorizeAccess
    ReserveInventory -.->|"&lt;&lt;include&gt;&gt;"| UpdateInventory
    RequestFulfillment -.->|"&lt;&lt;include&gt;&gt;"| AuthorizeAccess
    NotifyShipped -.->|"&lt;&lt;include&gt;&gt;"| SendNotification
    CancelOrder -.->|"&lt;&lt;include&gt;&gt;"| ProcessRefund
    CancelOrder -.->|"&lt;&lt;include&gt;&gt;"| UpdateInventory
    
    %% Extend Relationships
    ModifyOrder -.->|"&lt;&lt;extend&gt;&gt;"| CreateOrder
    CalculateTotal -.->|"&lt;&lt;extend&gt;&gt;"| CreateOrder
    TrackDelivery -.->|"&lt;&lt;extend&gt;&gt;"| TrackOrder
    CapturePOD -.->|"&lt;&lt;extend&gt;&gt;"| UpdateOrderStatus
    OptimizeRoute -.->|"&lt;&lt;extend&gt;&gt;"| RequestFulfillment
    GenerateShippingLabel -.->|"&lt;&lt;extend&gt;&gt;"| NotifyShipped
    GenerateReport -.->|"&lt;&lt;extend&gt;&gt;"| GetOrder
    PredictDemand -.->|"&lt;&lt;extend&gt;&gt;"| ReserveInventory
    
    %% Actor Connections to Use Cases
    Customer --> CreateOrder
    Customer --> GetOrder
    Customer --> CancelOrder
    Customer --> TrackOrder
    Customer --> ModifyOrder
    
    WarehouseManager --> GetOrder
    WarehouseManager --> UpdateOrderStatus
    WarehouseManager --> GenerateReport
    
    SystemAdmin --> AuthenticateUser
    SystemAdmin --> AuthorizeAccess
    SystemAdmin --> GenerateReport
    
    Picker --> RequestFulfillment
    Picker --> AuthorizeAccess
    
    Driver --> TrackDelivery
    Driver --> CapturePOD
    Driver --> AuthorizeAccess
    
    CustomerService --> GetOrder
    CustomerService --> CancelOrder
    CustomerService --> ProcessRefund
    CustomerService --> SendNotification
    
    %% External Service Connections (shown as system boundaries)
    subgraph "External Services"
        FraudDetectionService["Fraud Detection Service"]
        InventoryService["Inventory Management Service"]
        FulfillmentService["Picking & Packing Engine"]
        ShippingService["Shipping & Manifest Service"]
        NotificationService["Customer Notification Service"]
        PaymentService["Billing & Payment Subsystem"]
        IAMService["Identity & Access Management"]
        ProductCatalogService["Product Catalog Service"]
        RoutePlanningService["Route Planning Engine"]
        DeliveryTrackingService["Delivery Tracking Service"]
        AnalyticsService["Analytics & Reporting"]
        PredictiveService["Predictive Intelligence Engine"]
        ReturnsService["Reverse Logistics Module"]
    end
    
    %% Connections to External Services
    CheckFraud --> FraudDetectionService
    ReserveInventory --> InventoryService
    UpdateInventory --> InventoryService
    RequestFulfillment --> FulfillmentService
    NotifyShipped --> ShippingService
    SendNotification --> NotificationService
    ProcessPayment --> PaymentService
    ProcessRefund --> PaymentService
    AuthenticateUser --> IAMService
    AuthorizeAccess --> IAMService
    GetProductInfo --> ProductCatalogService
    OptimizeRoute --> RoutePlanningService
    TrackDelivery --> DeliveryTrackingService
    GenerateReport --> AnalyticsService
    PredictDemand --> PredictiveService
    ProcessRefund --> ReturnsService
    
    %% Styling
    classDef actorClass fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef useCaseClass fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef externalServiceClass fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef includeRel fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px
    classDef extendRel fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    
    class Customer,WarehouseManager,SystemAdmin,Picker,Driver,CustomerService actorClass
    class CreateOrder,GetOrder,CancelOrder,TrackOrder,UpdateOrderStatus,ReserveInventory,ValidateOrder,ProcessPayment,CalculateTotal,ModifyOrder useCaseClass
    class FraudDetectionService,InventoryService,FulfillmentService,ShippingService,NotificationService,PaymentService,IAMService,ProductCatalogService,RoutePlanningService,DeliveryTrackingService,AnalyticsService,PredictiveService,ReturnsService externalServiceClass
```

## Use Case Diagram Explanation

### Primary Actors
1. **Customer** - End user placing and tracking orders
2. **Warehouse Manager** - Oversees order processing and fulfillment
3. **System Administrator** - Manages system access and security
4. **Picker** - Warehouse worker fulfilling orders
5. **Driver** - Delivery personnel
6. **Customer Service Rep** - Handles customer inquiries and issues

### Core Use Cases
1. **Create Order** - Primary use case for order creation
2. **Get Order Details** - Retrieve order information
3. **Cancel Order** - Cancel existing orders
4. **Track Order Status** - Monitor order progress
5. **Update Order Status** - Change order state
6. **Reserve Inventory** - Allocate stock for orders

### Include Relationships (Required)
- **Validate Order** is included in Create Order
- **Check for Fraud** is included in Create Order
- **Reserve Inventory** is included in Create Order
- **Process Payment** is included in Create Order
- **Authenticate User** is included in Create Order
- **Authorize Access** is included in Create Order
- **Get Product Information** is included in Create Order

### Extend Relationships (Optional)
- **Modify Order** extends Create Order (for order changes)
- **Calculate Total** extends Create Order (for pricing)
- **Track Delivery** extends Track Order (for delivery tracking)
- **Capture Proof of Delivery** extends Update Order Status
- **Optimize Route** extends Request Fulfillment
- **Generate Shipping Label** extends Notify Shipped
- **Generate Report** extends Get Order (for analytics)
- **Predict Demand** extends Reserve Inventory (for forecasting)

### External Service Connections
The Order Management Service connects to all 19 other modules:

1. **Identity & Access Management (Nidhi)** - Authentication and authorization
2. **Product Catalog Service (Shivani)** - Product information
3. **Inventory Management & Location Service (Joshua)** - Stock management
4. **Picking & Packing Orchestration Engine (Kushi)** - Fulfillment
5. **Shipping & Manifest Service (Rahul)** - Shipping operations
6. **Reverse Logistics Module (Aadi)** - Returns processing
7. **Customer Notification & Proof of Delivery Service (John)** - Customer communications
8. **Analytics & Reporting Dashboard (Prasham)** - Business intelligence
9. **Billing and Payment Subsystem (Aditya Vilas)** - Financial transactions
10. **Fleet & Vehicle Management Service (Aadi)** - Delivery fleet
11. **Route Planning & Optimization Engine (Megh)** - Delivery optimization
12. **Real-Time Delivery Tracking & Driver Interface (Swastik)** - Delivery tracking
13. **Customer Relations Module (Sameet)** - Customer support
14. **Master Robotics Control & Task Dispatcher (Purva)** - Automation control
15. **Autonomous Mobile Robot Control System (Saransh)** - AMR operations
16. **Automated Storage/Retrieval Gateway (Arjun)** - AS/RS operations
17. **Aerial Drone Inventory Auditing System (Madhav)** - Inventory auditing
18. **Predictive Intelligence Engine (Tushar)** - AI/ML predictions
19. **Inbound Logistics & Receiving Module (Riddhimman)** - Receiving operations

This comprehensive use case diagram demonstrates how the Order Management Service serves as the central orchestration hub, connecting to every other module in the smart warehouse ecosystem while maintaining clear include and extend relationships for proper use case modeling.