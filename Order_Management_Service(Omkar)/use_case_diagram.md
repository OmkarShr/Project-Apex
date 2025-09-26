@startuml
title Order Management Service Use Case Diagram

!define ACTOR_COLOR #e1f5fe
!define USE_CASE_COLOR #f3e5f5
!define EXTERNAL_SERVICE_COLOR #fff3e0
!define INCLUDE_COLOR #c8e6c9
!define EXTEND_COLOR #ffcdd2

skinparam actor {
    BackgroundColor ACTOR_COLOR
    BorderColor #01579b
    BorderThickness 2
}

skinparam usecase {
    BackgroundColor USE_CASE_COLOR
    BorderColor #4a148c
    BorderThickness 2
}

skinparam package {
    BackgroundColor EXTERNAL_SERVICE_COLOR
    BorderColor #e65100
    BorderThickness 2
}

' Actors
:Customer: as Customer
:Warehouse Manager: as WarehouseManager
:System Administrator: as SystemAdmin
:Picker: as Picker
:Driver: as Driver
:Customer Service Rep: as CustomerService

' Core Use Cases
rectangle "Order Management Service" {
    (Create Order) as CreateOrder
    (Get Order Details) as GetOrder
    (Cancel Order) as CancelOrder
    (Track Order Status) as TrackOrder
    (Update Order Status) as UpdateOrderStatus
    (Reserve Inventory) as ReserveInventory
    (Validate Order) as ValidateOrder
    (Process Payment) as ProcessPayment
    (Calculate Order Total) as CalculateTotal
    (Modify Order) as ModifyOrder
}

' External Service Use Cases
(Check for Fraud) as CheckFraud
(Request Fulfillment) as RequestFulfillment
(Notify Shipped) as NotifyShipped
(Update Inventory) as UpdateInventory
(Generate Shipping Label) as GenerateShippingLabel
(Send Customer Notification) as SendNotification
(Process Refund) as ProcessRefund
(Authenticate User) as AuthenticateUser
(Authorize Access) as AuthorizeAccess
(Get Product Information) as GetProductInfo
(Optimize Delivery Route) as OptimizeRoute
(Track Delivery) as TrackDelivery
(Capture Proof of Delivery) as CapturePOD
(Generate Analytics Report) as GenerateReport
(Predict Demand) as PredictDemand

' External Services Package
package "External Services" {
    [Fraud Detection Service] as FraudDetectionService
    [Inventory Management Service] as InventoryService
    [Picking & Packing Engine] as FulfillmentService
    [Shipping & Manifest Service] as ShippingService
    [Customer Notification Service] as NotificationService
    [Billing & Payment Subsystem] as PaymentService
    [Identity & Access Management] as IAMService
    [Product Catalog Service] as ProductCatalogService
    [Route Planning Engine] as RoutePlanningService
    [Delivery Tracking Service] as DeliveryTrackingService
    [Analytics & Reporting] as AnalyticsService
    [Predictive Intelligence Engine] as PredictiveService
    [Reverse Logistics Module] as ReturnsService
}

' Actor to Use Case Relationships
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

' Include Relationships
CreateOrder .> ValidateOrder : <<include>>
CreateOrder .> CheckFraud : <<include>>
CreateOrder .> ReserveInventory : <<include>>
CreateOrder .> ProcessPayment : <<include>>
CreateOrder .> AuthenticateUser : <<include>>
CreateOrder .> AuthorizeAccess : <<include>>
CreateOrder .> GetProductInfo : <<include>>

UpdateOrderStatus .> AuthorizeAccess : <<include>>
ReserveInventory .> UpdateInventory : <<include>>
RequestFulfillment .> AuthorizeAccess : <<include>>
NotifyShipped .> SendNotification : <<include>>
CancelOrder .> ProcessRefund : <<include>>
CancelOrder .> UpdateInventory : <<include>>

' Extend Relationships
ModifyOrder ..> CreateOrder : <<extend>>
CalculateTotal ..> CreateOrder : <<extend>>
TrackDelivery ..> TrackOrder : <<extend>>
CapturePOD ..> UpdateOrderStatus : <<extend>>
OptimizeRoute ..> RequestFulfillment : <<extend>>
GenerateShippingLabel ..> NotifyShipped : <<extend>>
GenerateReport ..> GetOrder : <<extend>>
PredictDemand ..> ReserveInventory : <<extend>>

' External Service Connections
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
