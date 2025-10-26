@startuml
left to right direction
skinparam actorStyle awesome
skinparam usecase {
BorderColor #444
BackgroundColor #F9FAFB
}

' External actors
actor "Customer" as Customer
actor "Supplier" as Supplier
actor "Carrier API" as Carrier
actor "Payment Gateway" as PayGW

' Team actors (all separate as requested)
actor "Nidhi (IAM)" as Nidhi
actor "Shivani (Product Catalog)" as Shivani
actor "Riddhimman (Inbound)" as Riddhi
actor "Joshua (Inventory)" as Joshua
actor "Omkar (Order Mgmt)" as Omkar
actor "Kushi (Pick/Pack)" as Kushi
actor "Rahul (Shipping)" as Rahul
actor "Aadi (Returns)" as AadiR
actor "Purva (Robotics Dispatch)" as Purva
actor "Saransh (AMR Control)" as Saransh
actor "Arjun (AS/RS)" as Arjun
actor "Sameet (Cust Relations)" as Sameet
actor "Madhav (UAV Audit)" as Madhav
actor "Aadi (Fleet Mgmt)" as AadiF
actor "Megh (Route Planning)" as Megh
actor "Swastik (Driver/Tracking)" as Swastik
actor "John (Notification/POD)" as John
actor "Prasham (Analytics)" as Prasham
actor "Tushar (AI/ML)" as Tushar
actor "Aditya (Billing)" as Aditya

' Groups
rectangle "Access & Master Data" {
usecase "1) Identity & Access Mgmt" as UC1
usecase "2) Product Catalog" as UC2
}

rectangle "Core Fulfillment" {
usecase "3) Inbound & Receiving" as UC3
usecase "4) Inventory Mgmt & Locations" as UC4
usecase "5) Order Management" as UC5
usecase "6) Pick & Pack" as UC6
usecase "7) Shipping & Manifest" as UC7
usecase "8) Reverse Logistics" as UC8
}

rectangle "Automation & Last Mile" {
usecase "9) Robotics Dispatcher" as UC9
usecase "10) AMR Control" as UC10
usecase "11) AS/RS Gateway" as UC11
usecase "13) UAV Inventory Audit" as UC13
usecase "14) Fleet & Vehicles" as UC14
usecase "15) Route Optimization" as UC15
usecase "16) Delivery Tracking & Driver App" as UC16
usecase "17) Customer Notification & POD" as UC17
}

rectangle "Insights & Finance" {
usecase "12) Customer Relations" as UC12
usecase "18) Analytics Dashboard" as UC18
usecase "19) Predictive Intelligence" as UC19
usecase "20) Billing & Payments" as UC20
}

' Ownership links (one per actor to their module)
Nidhi -- UC1
Shivani -- UC2
Riddhi -- UC3
Joshua -- UC4
Omkar -- UC5
Kushi -- UC6
Rahul -- UC7
AadiR -- UC8
Purva -- UC9
Saransh -- UC10
Arjun -- UC11
Sameet -- UC12
Madhav -- UC13
AadiF -- UC14
Megh -- UC15
Swastik -- UC16
John -- UC17
Prasham -- UC18
Tushar -- UC19
Aditya -- UC20

' External actors to key modules
Supplier -- UC3
Customer -- UC12
Customer -- UC17
Carrier -- UC7
PayGW -- UC20

' Minimal essential flows
UC1 ..> UC5 : <<authorize>>
UC1 ..> UC4 : <<authorize>>
UC1 ..> UC7 : <<authorize>>

UC2 <.. UC3 : <<include>>\nSKU validation
UC3 --> UC4 : <<include>>\nputaway stock

UC5 --> UC4 : <<include>>\nreserve stock
UC5 --> UC6 : <<include>>\ncreate picks
UC5 --> UC20 : <<include>>\npayment auth
UC6 .> UC7 : <<extend>>\nready to ship

UC7 --> UC5 : <<include>>\ntracking update
UC7 --> UC20 : <<include>>\nfinalize bill

UC8 --> UC4 : <<include>>\nrestock/hold
UC8 --> UC20 : <<include>>\nrefund

UC6 --> UC9 : <<include>>\nrobotic tasks
UC9 --> UC10 : <<include>>\nAMR missions
UC9 --> UC11 : <<include>>\nAS/RS ops
UC13 --> UC4 : <<include>>\naudit variances

UC14 --> UC15 : <<include>>\nvehicle constraints
UC15 --> UC16 : <<include>>\nroutes to app
UC16 --> UC17 : <<include>>\nETA & POD
UC16 --> UC5 : <<include>>\nstatus updates

UC12 --> UC5 : <<include>>\norder context
UC12 --> UC7 : <<include>>\nshipping info

UC18 <.. UC4 : <<include>>\nstock KPIs
UC18 <.. UC6 : <<include>>\npick KPIs
UC18 <.. UC7 : <<include>>\nship KPIs
UC18 <.. UC16 : <<include>>\nOTD KPIs
UC18 <.. UC20 : <<include>>\nfinance KPIs

UC19 --> UC4 : <<include>>\ndemand signals
UC19 --> UC14 : <<include>>\npredictive maintenance
UC19 --> UC18 : <<include>>\npredictive insights

UC20 --> PayGW : capture/settlement
@enduml
