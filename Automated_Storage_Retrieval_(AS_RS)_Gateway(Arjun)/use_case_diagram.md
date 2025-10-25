@startuml
' ======================
' Style / skin settings
' ======================
skinparam monochrome false
skinparam shadowing true
skinparam packageStyle rectangle
skinparam actorStyle awesome

' ======================
' Actors (external systems / team modules)
' ======================
actor "Master Robotics\nControl Dispatcher" as Dispatcher #LightBlue
actor "AS/RS Hardware\n(Cranes / Shuttles / VLMs)" as ASRS_HW #LightSalmon

' Group and label team modules by domain (these act as external systems)
package "Core Modules" {
  actor "IAM Service\n(Identity & Access Management)" as IAM #LightGray
  actor "Product Catalog Service" as ProductCatalog #LightGray
  actor "Inbound Logistics\n& Receiving" as Inbound #LightGray
  actor "Inventory Mgmt &\nLocation Service" as InventorySvc #LightGray
  actor "Order Management\nService" as OrderMgmt #LightGray
}

package "Fulfillment & Operations" {
  actor "Picking & Packing\nOrchestration" as Picking #BurlyWood
  actor "Shipping & Manifest" as Shipping #BurlyWood
  actor "Reverse Logistics\n(Returns)" as ReverseLog #BurlyWood
}

package "Automation & Robotics" {
  actor "Master Robotics\nControl & Task\nDispatcher (team)" as TeamDispatcher #LightBlue
  actor "AMR Control System" as AMR #LightBlue
  actor "UAV Inventory\nAuditing System" as UAV #LightBlue
}

package "Customer & Delivery" {
  actor "Customer Relations" as CRM #PaleGreen
  actor "Fleet & Vehicle\nManagement" as Fleet #PaleGreen
  actor "Route Planning\n& Optimization" as Route #PaleGreen
  actor "Real-time Delivery\nTracking & Driver UI" as Realtime #PaleGreen
  actor "Customer Notification\n& PoD" as Notif #PaleGreen
}

package "Analytics & Intelligence" {
  actor "Analytics &\nReporting Dashboard" as Analytics #LightYellow
  actor "Predictive Intelligence\n(AI/ML)" as Predictive #LightYellow
  actor "Billing & Payment\nSubsystem" as Billing #LightYellow
}

' ======================
' System boundary - My Code (Software Gateway)
' ======================
rectangle "Software Gateway\n(standardized gateway - my code)" as Gateway {
  usecase "Store Item\n(storeItem(binId,location))" as UC_Store
  usecase "Retrieve Item\n(retrieveItem(binId))" as UC_Retrieve
  usecase "Translate Command\n(map to vendor protocol)" as UC_Translate
  usecase "Adapt to Vendor\nProtocol (upgrade/replace)" as UC_Adapt
  usecase "Manage System State\n(track item locations, equipment state)" as UC_ManageState
  usecase "Monitor & Confirm\n(monitor jobs, confirm results)" as UC_Monitor
  usecase "Detect & Report Error\n(error detection & forwarding)" as UC_Error
  usecase "Handle Movement / Execute\n(move/store/retrieve operations)" as UC_Handle
  usecase "Communicate with\nExternal Modules" as UC_Communicate
}

' ======================
' Primary actor interactions
' ======================
Dispatcher --> UC_Store : "sendCommand(storeItem)"
Dispatcher --> UC_Retrieve : "sendCommand(retrieveItem)"
Dispatcher --> UC_ManageState : "queryState() / subscribe status"
Dispatcher --> UC_Adapt : "request adapt/upgrade"

ASRS_HW --> UC_Handle : "low-level hardware responses\n(status, telemetry)"
ASRS_HW <-- UC_Translate : "translated vendor command"

' Gateway internal behavior relationships (include / extend)
' - common translation included by store/retrieve/handle
UC_Store .> UC_Translate : <<include>>
UC_Retrieve .> UC_Translate : <<include>>
UC_Handle .> UC_Translate : <<include>>

' - Monitoring always includes state management
UC_Monitor .> UC_ManageState : <<include>>

' - Error handling can extend monitor or store/retrieve during faults
UC_Error .> UC_Monitor : <<include>>
UC_Error .> UC_Handle : <<extend>> : "on operation failure"
UC_Error .> UC_Store : <<extend>> : "on store failure"
UC_Error .> UC_Retrieve : <<extend>> : "on retrieve failure"

' - Adaptation is an optional flow for vendor changes
UC_Adapt .> UC_Translate : <<include>>

' - Confirmation is an optional extension of Monitor
usecase "Confirm Task Completion" as UC_Confirm
UC_Confirm .> UC_Monitor : <<include>>
UC_Confirm .> UC_ManageState : <<include>>
UC_Confirm .> UC_Handle : <<extend>> : "on successful execution"

' ======================
' Connections between Gateway and team modules (highlighted)
' ======================
' These show where your gateway integrates with team members (clear integration points)
UC_Communicate <-- InventorySvc : "update inventory location\n(get/put confirmations)"
UC_Communicate <-- OrderMgmt : "order-driven store/retrieve\n(task creation)"
UC_Communicate <-- Picking : "task assignment / pick confirmations"
UC_Communicate <-- Shipping : "handoff notifications / manifests"
UC_Communicate <-- Inbound : "incoming receipt -> store tasks"
UC_Communicate <-- ProductCatalog : "item metadata lookup"
UC_Communicate <-- IAM : "authn / authz for commands"
UC_Communicate <-- Analytics : "telemetry / KPI events"
UC_Communicate <-- Predictive : "feed ML signals / receive optimizations"
UC_Communicate <-- Billing : "billing events for moved items"
UC_Communicate <-- CRM : "customer notifications (status)"
UC_Communicate <-- Fleet : "handoff to delivery / tracking info"
UC_Communicate <-- Route : "ETA info / delivery orchestration"
UC_Communicate <-- Realtime : "delivery state push / POD sync"
UC_Communicate <-- Notif : "final customer notifications"
UC_Communicate <-- AMR : "coordinate non-ASRS moves"
UC_Communicate <-- UAV : "inventory audits -> reconcile locations"
UC_Communicate <-- ReverseLog : "returns processing -> store or dispose"

' Highlight these integration connectors with notes for clarity
note right of UC_Communicate
  Integration points (APIs / Events / RPC):
  - InventorySvc: Inventory updates (REST / event)
  - OrderMgmt: Task creation requests
  - IAM: OAuth/OIDC or token-based auth
  - Analytics / Predictive: telemetry & feedback loop
  These are the connections between *your code* (Gateway) and team modules.
end note

' ======================
' Show "my code" vs "team code" mapping with colored note
' ======================
note left of Gateway
  Gateway = *your code* (translator + state manager).
  Key responsibilities:
   • Accept simple commands from Master Dispatcher
   • Translate to vendor protocols (UC_Translate)
   • Track state & confirm (UC_ManageState / UC_Monitor)
   • Report errors (UC_Error)
   • Communicate with team modules via standardized APIs (UC_Communicate)
end note

' ======================
' Visual separators for team responsibilities (optional)
' ======================
note bottom of Dispatcher
  Master Robotics Dispatcher:
  - external orchestrator that sends high-level commands
end note

' ======================
' End
' ======================
@enduml
