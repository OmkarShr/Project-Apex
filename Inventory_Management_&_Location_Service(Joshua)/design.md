# Inventory Management & Location Service Design (Revised)

## 1. Classes and Member Methods

### Class: `InventoryService`

The main service class for managing inventory.

| Member Method | Purpose |
| :--- | :--- |
| `add_item_to_inventory(sku, quantity, location_id)` | Adds a new item to the inventory. |
| `get_inventory_item(item_id)` | Retrieves an inventory item. |
| `get_item_location(sku)` | Retrieves the location of an item. |
| `update_stock_level(sku, change, reason)` | Updates the stock level of a product. |
| `move_item(item_id, new_location_id)` | Moves an item to a new location. |
| `get_items_at_location(location_id)` | Retrieves all items at a specific location. |

### Class: `InventoryItem`

Represents a specific item in the inventory.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(item_id, sku, location, quantity, status)` | Initializes a new InventoryItem object. |
| `update_quantity(new_quantity)` | Updates the quantity of the item. |
| `update_status(new_status)` | Updates the status of the item. |

### Class: `WarehouseLocation`

Represents a location in the warehouse.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(location_id, location_hierarchy)` | Initializes a new WarehouseLocation object. |
| `get_full_location_string()` | Returns the full location string. |

### Class: `LocationHierarchy`

Represents the hierarchy of locations (zone, aisle, rack, bin).

| Member Method | Purpose |
| :--- | :--- |
| `__init__(zone, aisle, rack, bin)` | Initializes a new LocationHierarchy object. |

### Class: `InventoryTransaction`

Represents a transaction that affects inventory levels.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(transaction_id, item_id, change_amount, reason)` | Initializes a new InventoryTransaction object. |

### Class: `SlottingService`

A service for optimizing item placement.

| Member Method | Purpose |
| :--- | :--- |
| `recommend_location(sku)` | Recommends an optimal location for a given SKU. |
| `analyze_product_velocity(sku)` | Analyzes the sales velocity of a product. |

### Class: `Movement`

Represents the movement of an item from one location to another.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(movement_id, item_id, from_location, to_location, timestamp)` | Initializes a new Movement object. |

### Class: `InventoryStatus`

Represents the status of an inventory item.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(status_id, status_name)` | Initializes a new InventoryStatus object. |