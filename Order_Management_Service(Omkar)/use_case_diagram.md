@startuml Order Management Service Use Case Diagram

skinparam actor {
    BackgroundColor #e1f5fe
    BorderColor #01579b
    BorderThickness 2
}

skinparam usecase {
    BackgroundColor #f3e5f5
    BorderColor #4a148c
    BorderThickness 2
}

skinparam package {
    BackgroundColor #fff3e0
    BorderColor #e65100
    BorderThickness 2
}

actor Customer
actor "Joshua (Warehouse Manager)" as Joshua
actor "Nidhi (System Admin)" as Nidhi
actor "Kushi (Picker)" as Kushi
actor "Swastik (Driver)" as Swastik
actor "Sameet (Customer Service)" as Sameet
actor "Rahul (Shipping Manager)" as Rahul
actor "Aditya Vilas (Payment Manager)" as AdityaVilas
actor "Prasham (Analytics Manager)" as Prasham
actor "Purva (Robotics Manager)" as Purva
actor "Arjun (AS/RS Manager)" as Arjun
actor "Madhav (Drone Manager)" as Madhav

rectangle "Order Management Service" {
    usecase "Create Order" as CreateOrder
    usecase "Get Order Details" as GetOrder
    usecase "Cancel Order" as CancelOrder
    usecase "Track Order Status" as TrackOrder
    usecase "Update Order Status" as UpdateOrderStatus
    usecase "Reserve Inventory" as ReserveInventory
    usecase "Validate Order" as ValidateOrder
    usecase "Process Payment" as ProcessPayment
    usecase "Calculate Order Total" as CalculateTotal
    usecase "Modify Order" as ModifyOrder
}

usecase "Check for Fraud" as CheckFraud
usecase "Request Fulfillment" as RequestFulfillment
usecase "Notify Shipped" as NotifyShipped
usecase "Update Inventory" as UpdateInventory
usecase "Generate Shipping Label" as GenerateShippingLabel
usecase "Send Customer Notification" as SendNotification
usecase "Process Refund" as ProcessRefund
usecase "Authenticate User" as AuthenticateUser
usecase "Authorize Access" as AuthorizeAccess
usecase "Get Product Information" as GetProductInfo
usecase "Optimize Delivery Route" as OptimizeRoute
usecase "Track Delivery" as TrackDelivery
usecase "Capture Proof of Delivery" as CapturePOD
usecase "Generate Analytics Report" as GenerateReport
usecase "Predict Demand" as PredictDemand

package "External Services" {
    component [Fraud Detection Service] as FraudDetectionService
    component [Inventory Management Service] as InventoryService
    component [Picking & Packing Engine] as FulfillmentService
    component [Shipping & Manifest Service] as ShippingService
    component [Customer Notification Service] as NotificationService
    component [Billing & Payment Subsystem] as PaymentService
    component [Identity & Access Management] as IAMService
    component [Product Catalog Service] as ProductCatalogService
    component [Route Planning Engine] as RoutePlanningService
    component [Delivery Tracking Service] as DeliveryTrackingService
    component [Analytics & Reporting] as AnalyticsService
    component [Predictive Intelligence Engine] as PredictiveService
    component [Reverse Logistics Module] as ReturnsService
}

Customer --> CreateOrder
Customer --> GetOrder
Customer --> CancelOrder
Customer --> TrackOrder
Customer --> ModifyOrder

Joshua --> GetOrder
Joshua --> UpdateOrderStatus
Joshua --> GenerateReport

Nidhi --> AuthenticateUser
Nidhi --> AuthorizeAccess
Nidhi --> GenerateReport

Kushi --> RequestFulfillment
Kushi --> AuthorizeAccess

Swastik --> TrackDelivery
Swastik --> CapturePOD
Swastik --> AuthorizeAccess

Sameet --> GetOrder
Sameet --> CancelOrder
Sameet --> ProcessRefund
Sameet --> SendNotification

Rahul --> NotifyShipped
Rahul --> GenerateShippingLabel

AdityaVilas --> ProcessPayment
AdityaVilas --> ProcessRefund

Prasham --> GenerateReport

Purva --> RequestFulfillment
Purva --> AuthorizeAccess

Arjun --> RequestFulfillment
Arjun --> AuthorizeAccess

Madhav --> UpdateInventory
Madhav --> AuthorizeAccess

CreateOrder .> ValidateOrder : include
CreateOrder .> CheckFraud : include
CreateOrder .> ReserveInventory : include
CreateOrder .> ProcessPayment : include
CreateOrder .> AuthenticateUser : include
CreateOrder .> AuthorizeAccess : include
CreateOrder .> GetProductInfo : include

UpdateOrderStatus .> AuthorizeAccess : include
ReserveInventory .> UpdateInventory : include
RequestFulfillment .> AuthorizeAccess : include
NotifyShipped .> SendNotification : include
CancelOrder .> ProcessRefund : include
CancelOrder .> UpdateInventory : include

ModifyOrder .> CreateOrder : extend
CalculateTotal .> CreateOrder : extend
TrackDelivery .> TrackOrder : extend
CapturePOD .> UpdateOrderStatus : extend
OptimizeRoute .> RequestFulfillment : extend
GenerateShippingLabel .> NotifyShipped : extend
GenerateReport .> GetOrder : extend
PredictDemand .> ReserveInventory : extend

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

@enduml
