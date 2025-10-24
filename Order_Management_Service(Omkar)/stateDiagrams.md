

# Order Management Service — State Charts
---
## 1. Order Lifecycle
```mermaid
stateDiagram-v2
[*] --> Order_Created: Customer places order
state Order {
Order_Created --> Validated: validate_order()
Validated --> Payment_Pending: request_payment()
Validated --> Cancelled: validation_failed / notify_customer
Payment_Pending --> Payment_Success: payment_success
Payment_Pending --> Payment_Failed: payment_failed
Payment_Success --> Fork_PostPayment
Payment_Failed --> Cancelled: cancel_order()
Fork_PostPayment --> Inventory_Reserve: reserve_inventory
Fork_PostPayment --> Fulfillment_Prepare: create_fulfillment_task
Inventory_Reserve --> Join_PostProcessing
Fulfillment_Prepare --> Join_PostProcessing
Join_PostProcessing --> Awaiting_Fulfillment: all_subtasks_complete
Awaiting_Fulfillment --> Shipped: handed_to_carrier
Shipped --> Delivered: delivered_to_customer
Delivered --> Completed: confirm_delivery / auto_confirm
Cancelled --> [*]
Completed --> [*]
}
note right of Fork_PostPayment
Fork: inventory + fulfillment run in parallel
Join: both must succeed to proceed
end note
```
---
## 2. Payment Processing
```mermaid
stateDiagram-v2
[*] --> Payment_Initiated: payment requested
state Payment {
Payment_Initiated --> Authorizing: send_authorization
Authorizing --> Authorized: auth_success
Authorizing --> Declined: auth_declined
Authorized --> AwaitingCapture: hold_placed
AwaitingCapture --> Capturing: capture_triggered
Capturing --> Captured: capture_success
Capturing --> CaptureFailed: capture_failed
Declined --> Payment_Failed: notify_customer
CaptureFailed --> Payment_Failed: escalate / refund
Captured --> Payment_Completed
Payment_Completed --> [*]
}
note right of Authorizing
Retries and gateway-timeouts handled with backoff
end note
```
---
## 3. Inventory Reservation
```mermaid
stateDiagram-v2
[*] --> Reservation_Requested: after payment success
state Inventory {
Reservation_Requested --> Checking_Availability: query_inventory
Checking_Availability --> All_Available: all_in_stock
Checking_Availability --> Partial_Available: partial
Checking_Availability --> Out_Of_Stock: none_available
All_Available --> Reserving_Stock: begin_reservation
Reserving_Stock --> Fork_Reserve
Fork_Reserve --> Lock_Records
Fork_Reserve --> Create_Reservation_Record
Fork_Reserve --> Update_StockCounts
Lock_Records --> Join_Reserve
Create_Reservation_Record --> Join_Reserve
Update_StockCounts --> Join_Reserve
Join_Reserve --> Reserved: reservation_success
Out_Of_Stock --> Reservation_Failed: notify_customer
Partial_Available --> AwaitingCustomerDecision: offer_partial
Reserved --> Allocated: order_fulfilled
Reserved --> Released: order_cancelled_or_expired
}
note right of Fork_Reserve
Parallel reservation steps (lock, record, stock update)
Join: commit or rollback as one unit
end note
```
---
## 4. Order Fulfillment
```mermaid
stateDiagram-v2
[*] --> Fulfillment_Requested: inventory reserved
state Fulfillment {
Fulfillment_Requested --> Warehouse_Assigned: select_warehouse
Warehouse_Assigned --> Picking_Queued: notify_warehouse
Picking_Queued --> Picking_In_Progress: picker_assigned
Picking_In_Progress --> Fork_Picking
Fork_Picking --> Item_Pick
Fork_Picking --> Quality_Monitor
Fork_Picking --> Inventory_Update
Item_Pick --> Join_Picking
Quality_Monitor --> Join_Picking
Inventory_Update --> Join_Picking
Join_Picking --> Packing: items_ready
Packing --> QC: quality_check
QC --> ReadyForShipment: passed
ReadyForShipment --> HandedToCarrier: carrier_pickup
HandedToCarrier --> InTransit
}
note right of Fork_Picking
Parallel: picking + QC monitoring + inventory realtime updates
end note
```
---
## 5. Returns & Cancellation
```mermaid
stateDiagram-v2
[*] --> Return_Initiated: customer requests return
state Returns {
Return_Initiated --> Validate_Return: check_eligibility
Validate_Return --> Approved: within_policy
Validate_Return --> Rejected: not_eligible
Approved --> Fork_ReturnProcessing
Fork_ReturnProcessing --> ArrangePickup
Fork_ReturnProcessing --> InitiateRefund
Fork_ReturnProcessing --> UpdateInventoryOnReturn
ArrangePickup --> Join_Return
InitiateRefund --> Join_Return
UpdateInventoryOnReturn --> Join_Return
Join_Return --> Return_Completed: all steps done
Rejected --> NotifyCustomer
}
note right of Fork_ReturnProcessing
Parallel refund + pickup arrangement + inventory updates
end note
```
