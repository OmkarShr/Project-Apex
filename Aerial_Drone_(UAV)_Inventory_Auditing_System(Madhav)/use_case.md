@startuml
skinparam style strictuml

' --- Color and Style Configuration ---
skinparam backgroundcolor #FFFFFF
skinparam Shadowing false
skinparam Actor {
    BackgroundColor #C6E2FF
    BorderColor #003366
}
skinparam Rectangle {
    BackgroundColor #F0F8FF
    BorderColor #4682B4
    FontColor #000000
}
skinparam usecase {
    BackgroundColor #FFFFFF
    BorderColor #4682B4
}
skinparam Arrow {
    Color #003366
}

' --- Define All 19 Actors from the list ---
actor "Master Robotics Control & Task Dispatcher" as MasterControl
actor "Inventory Management & Location Service" as IMS
actor "Autonomous Mobile Robot (AMR) Control System" as AMR
actor "Predictive Intelligence Engine (AI/ML)" as AI_ML
actor "Analytics & Reporting Dashboard" as Analytics
actor "Identity & Access Management (IAM) Service" as IAM
actor "Picking & Packing Orchestration Engine" as Picking
actor "Automated Storage/Retrieval (AS/RS) Gateway" as ASRS
actor "Inbound Logistics & Receiving Module" as Inbound
actor "Reverse Logistics (Returns) Module" as Returns

actor "Fulfillment & Operations" as Fulfillment
actor "Order Management Service" as OrderMgmt
actor "Product Catalog Service" as ProductCatalog
actor "Shipping & Manifest Service" as Shipping
actor "Customer Relations Module" as CRM
actor "Fleet & Vehicle Management Service" as VehicleFleet
actor "Route Planning & Optimization Engine" as RoutePlanner
actor "Real-Time Delivery Tracking & Driver Interface" as DeliveryTracking
actor "Customer Notification & Proof of Delivery Service" as CustNotify
actor "Billing and Payment Subsystem" as Billing

' --- Force Vertical Actor Layout ---
MasterControl -[hidden]down- IMS
IMS -[hidden]down- AMR
AMR -[hidden]down- AI_ML
AI_ML -[hidden]down- Analytics
Analytics -[hidden]down- IAM
IAM -[hidden]down- Picking
Picking -[hidden]down- ASRS
ASRS -[hidden]down- Inbound
Inbound -[hidden]down- Returns

Fulfillment -[hidden]down- OrderMgmt
OrderMgmt -[hidden]down- ProductCatalog
ProductCatalog -[hidden]down- Shipping
Shipping -[hidden]down- CRM
CRM -[hidden]down- VehicleFleet
VehicleFleet -[hidden]down- RoutePlanner
RoutePlanner -[hidden]down- DeliveryTracking
DeliveryTracking -[hidden]down- CustNotify
CustNotify -[hidden]down- Billing


' --- Define the Main System Boundary ---
rectangle "Aerial Drone (UAV) Inventory Auditing System" {
  ' --- Primary Use Cases ---
  usecase "Perform Audit Mission" as U1
  usecase "Process Audit Report" as U2
  usecase "Manage UAV Fleet Health" as U3
  usecase "Secure System Access" as U4

  ' --- Internal Use Cases ---
  usecase "Assign Optimal UAV" as U1_Inc1
  usecase "Execute Flight & Scan" as U1_Inc2
  usecase "Analyze Captured Data" as U1_Inc3
  usecase "Coordinate with AMR" as U1_Inc4
  usecase "Flag Discrepancy" as U1_Ext1
  usecase "Trigger Return to Base" as U1_Ext2
  usecase "Request Item Relocation" as U1_Ext3

  ' ** CRITICAL: Force vertical layout for ALL internal use cases **
  U1 -[hidden]down- U2
  U2 -[hidden]down- U3
  U3 -[hidden]down- U4
  U4 -[hidden]down- U1_Inc1
  U1_Inc1 -[hidden]down- U1_Inc2
  U1_Inc2 -[hidden]down- U1_Inc3
  U1_Inc3 -[hidden]down- U1_Inc4
  U1_Inc4 -[hidden]down- U1_Ext1
  U1_Ext1 -[hidden]down- U1_Ext2
  U1_Ext2 -[hidden]down- U1_Ext3
}

' --- Internal Use Case Relationships ---
U1 .> U1_Inc1 : <<include>>
U1 .> U1_Inc2 : <<include>>
U1 .> U1_Inc3 : <<include>>
U1 .> U1_Inc4 : <<include>>

U1_Ext1 .> U1_Inc3 : <<extend>>
U1_Ext2 .> U1_Inc2 : <<extend>>
U1_Ext3 .> U1_Ext1 : <<extend>>

' --- Logical Connections from External Actors (More diverse connections) ---
' Core Operations
MasterControl -- U1 : (Initiates Mission)
IMS -- U1 : (Provides Inventory Data)
AMR -- U1_Inc4 : (Coordinates Path)
ProductCatalog -- U1 : (Provides Item Data)
Inbound -- U1 : (Triggers Verification Audit)
Returns -- U1 : (Triggers Verification Audit)

' Security
IAM -- U4 : (Authenticates/Authorizes)

' Data, Analytics & Intelligence
U2 -- Analytics : (Sends Report Data)
U3 -- Analytics : (Sends Fleet Health KPIs)
U2 -- AI_ML : (Provides Data for Forecasting)
U3 -- AI_ML : (Provides Data for Predictive Maintenance)

' Warehouse Floor Actions
U1_Ext3 -- Picking : (Flags Discrepancy for Action)
U1_Ext3 -- ASRS : (Flags Discrepancy for Action)

' Business & Customer Systems (Consume processed data via reports)
U2 -- Fulfillment
U2 -- OrderMgmt
U2 -- Shipping
U2 -- CRM
U2 -- VehicleFleet
U2 -- RoutePlanner
U2 -- DeliveryTracking
U2 -- CustNotify
U2 -- Billing

@enduml
