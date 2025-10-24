@startuml
left to right direction
skinparam packageStyle rectangle
skinparam usecase {
  BackgroundColor White
  BorderColor Black
  ArrowColor Black
}

actor "Customer" as A1
actor "Driver" as A2
actor "Dispatcher / Ops" as A3
actor "Fleet Manager" as A4
actor "Finance / Billing" as A5
actor "Carrier / 3PL" as A6
actor "Admin / IAM" as A7

rectangle "Project Apex System" as SYS {
  ' === Core Modules ===
  usecase UC_IAM as "Authenticate & Authorize (IAM)"
  usecase UC_CAT as "Maintain Product Catalog"
  usecase UC_INB as "Inbound Logistics & Receiving"
  usecase UC_INV as "Manage Inventory & Locations"
  usecase UC_ORD as "Manage Orders"
  usecase UC_PICK as "Pick & Pack Orchestration"
  usecase UC_SHIP as "Create Shipment & Manifest"
  usecase UC_RET as "Process Returns"
  usecase UC_ROB as "Robotics Task Dispatch"
  usecase UC_AMR as "AMR Control"
  usecase UC_ASRS as "AS/RS Gateway Ops"
  usecase UC_UAV as "Drone Inventory Audits"
  usecase UC_CRM as "Customer Support & SLA"
  usecase UC_FLEET as "Fleet & Vehicle Management"
  usecase UC_ROUTE as "Route Planning & Optimization"

  ' === Swastik’s Last-Mile Domain ===
  usecase UC_TRACK as "Real-Time Delivery Tracking"
  usecase UC_DRIVER as "Driver Mobile Interface"
  usecase UC_POD as "Proof of Delivery"
  usecase UC_REROUTE as "Re-routing & Traffic Handling"
  usecase UC_OFFLINE as "Offline Sync & Error Handling"
  usecase UC_REVERSE as "Reverse Pickup Flow"
  usecase UC_COD as "COD / Payment Confirmation"
  usecase UC_EXCEPTION as "Delivery Exception Reporting"

  ' === Intelligence & Support ===
  usecase UC_AI as "Predictive Intelligence (AI/ML)"
  usecase UC_BI as "Analytics & Reporting Dashboard"
  usecase UC_PAY as "Billing & Payments"
}

' === Includes ===
UC_DRIVER ..> UC_IAM : <<include>>
UC_DRIVER ..> UC_TRACK : <<include>>
UC_DRIVER ..> UC_POD   : <<include>>
UC_DRIVER ..> UC_FLEET : <<include>>
UC_DRIVER ..> UC_PAY   : <<include>>
UC_DRIVER ..> UC_SHIP  : <<include>>
UC_DRIVER ..> UC_ORD   : <<include>>

UC_TRACK ..> UC_ROUTE    : <<include>>
UC_TRACK ..> UC_POD      : <<include>>
UC_TRACK ..> UC_OFFLINE  : <<include>>
UC_TRACK ..> UC_EXCEPTION: <<include>>

UC_ROUTE ..> UC_FLEET  : <<include>>
UC_ROUTE ..> UC_AI     : <<include>>

UC_ORD ..> UC_INV      : <<include>>
UC_PICK ..> UC_INV     : <<include>>
UC_SHIP ..> UC_ORD     : <<include>>
UC_RET  ..> UC_INV     : <<include>>

UC_ROB ..> UC_AMR      : <<include>>
UC_ROB ..> UC_ASRS     : <<include>>
UC_UAV ..> UC_INV      : <<include>>

UC_AI  ..> UC_TRACK    : <<include>>
UC_BI  ..> UC_AI       : <<include>>
UC_BI  ..> UC_PAY      : <<include>>
UC_BI  ..> UC_FLEET    : <<include>>
UC_BI  ..> UC_ORD      : <<include>>

' === Extends ===
UC_TRACK ..> UC_REROUTE  : <<extend>>
UC_TRACK ..> UC_REVERSE  : <<extend>>
UC_TRACK ..> UC_COD      : <<extend>>
UC_TRACK ..> UC_EXCEPTION: <<extend>>

UC_POD ..> UC_COD : <<extend>>
UC_POD ..> UC_RET : <<extend>>
UC_DRIVER ..> UC_REVERSE : <<extend>>
UC_DRIVER ..> UC_OFFLINE : <<extend>>
UC_ROUTE ..> UC_REROUTE : <<extend>>
UC_AI ..> UC_ROUTE : <<extend>>

' === Actor Links ===
A1 -- UC_POD
A1 -- UC_RET
A1 -- UC_CRM

A2 -- UC_DRIVER
A2 -- UC_TRACK
A2 -- UC_POD
A2 -- UC_REROUTE
A2 -- UC_REVERSE
A2 -- UC_COD
A2 -- UC_EXCEPTION

A3 -- UC_ROUTE
A3 -- UC_TRACK
A3 -- UC_CRM
A3 -- UC_PICK
A3 -- UC_SHIP

A4 -- UC_FLEET
A4 -- UC_ROUTE
A4 -- UC_TRACK

A5 -- UC_PAY
A5 -- UC_BI

A6 -- UC_SHIP
A6 -- UC_RET

A7 -- UC_IAM
A7 -- UC_CAT
A7 -- UC_BI

legend right
|= Module |= Owner |
| IAM | Nidhi |
| Product Catalog | Shivani |
| Inbound Logistics & Receiving | Riddhimman |
| Inventory Mgmt & Locations | Joshua |
| Order Management | Omkar |
| Picking & Packing | Kushi |
| Shipping & Manifest | Rahul |
| Reverse Logistics (Returns) | Aadi |
| Robotics Task Dispatch | Purva |
| AMR Control | Saransh |
| AS/RS Gateway | Arjun |
| UAV Inventory Audits | Madhav |
| Customer Relations (CRM) | Sameet |
| Fleet & Vehicle Mgmt | Aadi |
| Route Planning & Optimization | Megh |
| Real-Time Delivery Tracking & Driver Interface | Swastik |
| Customer Notification & POD | John |
| Analytics & Reporting Dashboard | Prasham |
| Predictive Intelligence (AI/ML) | Tushar |
| Billing & Payments | Aditya Vilas |
endlegend
@enduml
