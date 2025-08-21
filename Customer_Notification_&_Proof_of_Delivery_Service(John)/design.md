# Customer Notification & Proof of Delivery Service Design (Revised)

## 1. Classes and Member Methods

### Class: `NotificationService`

The main service for sending notifications.

| Member Method | Purpose |
| :--- | :--- |
| `send_notification(customer_id, message, channel)` | Sends a notification to a customer. |
| `subscribe_to_delivery_events()` | Subscribes to events from the Delivery Tracking service. |
| `notify_out_for_delivery(customer_id)` | Notifies a customer that their order is out for delivery. |
| `notify_eta_update(customer_id, new_eta)` | Notifies a customer of an updated ETA. |

### Class: `Notification`

Represents a notification to be sent to a customer.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(notification_id, customer_id, message, type)` | Initializes a new Notification object. |

### Class: `NotificationChannel`

An abstract class for different notification channels.

| Member Method | Purpose |
| :--- | :--- |
| `send(message)` | Sends a message through this channel. |

### Class: `SmsChannel`

A concrete implementation for the SMS channel.

| Member Method | Purpose |
| :--- | :--- |
| `send(message)` | Sends an SMS message. |

### Class: `EmailChannel`

A concrete implementation for the email channel.

| Member Method | Purpose |
| :--- | :--- |
| `send(message)` | Sends an email message. |

### Class: `ProofOfDeliveryService`

A service for managing proof of delivery.

| Member Method | Purpose |
| :--- | :--- |
| `capture_pod(order_id, pod_data)` | Captures the proof of delivery for an order. |
| `get_pod(order_id)` | Retrieves the proof of delivery for an order. |

### Class: `ProofOfDelivery`

Represents the proof of delivery for an order.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(pod_id, order_id, timestamp)` | Initializes a new ProofOfDelivery object. |
| `add_signature(signature)` | Adds a signature to the proof of delivery. |
| `add_photo(photo)` | Adds a photo to the proof of delivery. |

### Class: `Signature`

Represents a digital signature.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(signature_id, image_data)` | Initializes a new Signature object. |

### Class: `Photo`

Represents a photo taken as proof of delivery.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(photo_id, image_url)` | Initializes a new Photo object. |