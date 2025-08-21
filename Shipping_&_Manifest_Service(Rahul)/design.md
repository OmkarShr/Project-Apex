# Shipping & Manifest Service Design (Revised)

## 1. Classes and Member Methods

### Class: `ShippingService`

The main service for managing shipping and manifests.

| Member Method | Purpose |
| :--- | :--- |
| `create_shipment(order_id, carrier_id)` | Creates a new shipment for an order. |
| `get_shipment(shipment_id)` | Retrieves a shipment. |
| `get_shipping_rates(order_id)` | Gets shipping rates for an order from multiple carriers. |
| `select_best_rate(rates)` | Selects the best shipping rate based on predefined rules. |
| `generate_shipping_label(shipment_id)` | Generates a shipping label for a shipment. |
| `generate_manifest(carrier_id)` | Generates a manifest for a carrier. |

### Class: `Shipment`

Represents a shipment to be sent to a customer.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(shipment_id, order_id, carrier, tracking_number, label)` | Initializes a new Shipment object. |
| `get_tracking_url()` | Returns the tracking URL for the shipment. |

### Class: `Carrier`

Represents a shipping carrier.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(carrier_id, name, api_service)` | Initializes a new Carrier object. |

### Class: `Rate`

Represents a shipping rate.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(carrier, service_level, amount)` | Initializes a new Rate object. |

### Class: `Label`

Represents a shipping label.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(label_id, shipment_id, file_format, data)` | Initializes a new Label object. |

### Class: `Manifest`

Represents a manifest of all shipments for a carrier.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(manifest_id, carrier, shipments)` | Initializes a new Manifest object. |
| `add_shipment(shipment)` | Adds a shipment to the manifest. |

### Class: `CarrierApiService`

An abstract class for carrier API integrations.

| Member Method | Purpose |
| :--- | :--- |
| `get_rates(package_details)` | Gets shipping rates for a package. |
| `create_label(shipment_details)` | Creates a shipping label. |

### Class: `FedExApiService`

A concrete implementation for the FedEx API.

| Member Method | Purpose |
| :--- | :--- |
| `get_rates(package_details)` | Gets shipping rates from FedEx. |
| `create_label(shipment_details)` | Creates a FedEx shipping label. |

### Class: `UPSApiService`

A concrete implementation for the UPS API.

| Member Method | Purpose |
| :--- | :--- |
| `get_rates(package_details)` | Gets shipping rates from UPS. |
| `create_label(shipment_details)` | Creates a UPS shipping label. |

### Class: `USPSApiService`

A concrete implementation for the USPS API.

| Member Method | Purpose |
| :--- | :--- |
| `get_rates(package_details)` | Gets shipping rates from USPS. |
| `create_label(shipment_details)` | Creates a USPS shipping label. |