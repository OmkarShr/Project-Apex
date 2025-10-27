```mermaid
stateDiagram-v2
    [*] --> Receiving
    Receiving: Item being processed at intake
    
    Receiving --> Available : stock_approved()
    Receiving --> On_Quality_Hold : flag_for_inspection()

    Available: In location, ready for allocation
    Available --> Reserved : allocate_for_order()
    Available --> On_Quality_Hold : flag_for_cycle_count()
    Available --> Damaged : report_damage()

    Reserved: Allocated to an order, awaiting pick
    Reserved --> Picked : item_picked_by_amr()
    Reserved --> Available : order_cancelled() [de-allocate]

    On_Quality_Hold: Undergoing inspection
    On_Quality_Hold --> Available : inspection_passed()
    On_Quality_Hold --> Damaged : inspection_failed()

    Damaged: Marked for disposal/write-off
    Damaged --> [*] : item_written_off()

    Picked: Physically removed from shelf, in transit
    Picked --> [*] : item_shipped()
```
<br><br><br><br><br><br>
```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle: (Ready to accept transactions)
    
    Idle --> Processing_Transaction : (add_stock, move_item, query_item)
    Processing_Transaction: (Updating database, logging event)
    Processing_Transaction --> Idle : transaction_complete
    
    Processing_Transaction --> Error_State : transaction_failed
    Error_State --> Idle : error_logged

```
<br><br><br><br><br><br>
```mermaid
stateDiagram-v2
    [*] --> Pending
    Pending: Awaiting resource assignment (e.g., AMR)
    
    Pending --> In_Progress : assign_amr()
    In_Progress: Item is in transit
    
    In_Progress --> Completed : item_dropped_at_destination
    In_Progress --> Failed : move_failed (e.g., item not found)
    
    Completed --> [*]
    Failed --> [*]

```
