# Inbound Logistics & Receiving Module Design (Revised)

## 1. Classes and Member Methods

### Class: `InboundLogisticsService`

The main service class for managing inbound logistics.

| Member Method | Purpose |
| :--- | :--- |
| `process_asn(asn_data)` | Processes a new Advance Shipping Notice. |
| `get_asn(asn_id)` | Retrieves an ASN. |
| `schedule_dock_appointment(asn_id, appointment_time)` | Schedules a dock appointment for an ASN. |
| `initiate_receiving_session(asn_id, operator_id)` | Initiates a new receiving session. |

### Class: `ASN` (Advance Shipping Notice)

Represents an incoming shipment.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(asn_id, supplier_id, expected_arrival, line_items)` | Initializes a new ASN object. |
| `get_details()` | Returns the details of the ASN. |
| `add_line_item(line_item)` | Adds a line item to the ASN. |

### Class: `ASNDetail`

Represents a line item in an ASN.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(sku, quantity, unit_price)` | Initializes a new ASNDetail object. |

### Class: `Supplier`

Represents a supplier.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(supplier_id, name, contact_info)` | Initializes a new Supplier object. |

### Class: `DockSchedulingService`

A service for managing dock scheduling.

| Member Method | Purpose |
| :--- | :--- |
| `find_available_docks(time_window)` | Finds available docks in a given time window. |
| `book_dock(dock_id, time_window)` | Books a dock for a specific time window. |
| `get_schedule()` | Retrieves the dock schedule. |

### Class: `DockBooking`

Represents a booking for a dock.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(booking_id, dock_id, asn_id, start_time, end_time)` | Initializes a new DockBooking object. |

### Class: `ReceivingService`

A service for managing the receiving process.

| Member Method | Purpose |
| :--- | :--- |
| `start_session(asn_id, operator_id)` | Starts a new receiving session. |
| `receive_item(session_id, sku, quantity)` | Receives an item in a session. |
| `record_discrepancy(session_id, sku, expected_quantity, actual_quantity, reason)` | Records a discrepancy. |
| `end_session(session_id)` | Ends a receiving session. |

### Class: `ReceivingSession`

Represents a receiving session for an ASN.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(session_id, asn_id, operator_id, start_time)` | Initializes a new ReceivingSession object. |

### Class: `Discrepancy`

Represents a discrepancy found during receiving.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(discrepancy_id, session_id, sku, expected_quantity, actual_quantity, reason)` | Initializes a new Discrepancy object. |

### Class: `QualityControlService`

A service for managing quality control.

| Member Method | Purpose |
| :--- | :--- |
| `initiate_inspection(item_id, reason)` | Initiates an inspection for an item. |
| `record_inspection_result(inspection_id, result, notes)` | Records the result of an inspection. |

### Class: `Inspection`

Represents an inspection of an item.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(inspection_id, item_id, reason, status)` | Initializes a new Inspection object. |