classDiagram
   class Comparable {
       <<interface>>
       +compareTo(other:Object) int
   }


   class ShippingService {S
       +create_shipment(order_id, carrier_id): Shipment
       +get_shipment(shipment_id): Shipment
       +get_shipping_rates(order_id): list~Rate~
       +select_best_rate(rates): Rate
       +generate_shipping_label(shipment_id): Label
       +generate_manifest(carrier_id): Manifest
   }
   class Shipment {
       -shipment_id: string
       -order_id: string
       -carrier: Carrier
       -tracking_number: string
       -label: Label
       +__init__(shipment_id, order_id, carrier, tracking_number, label)
       +get_tracking_url(): string
       +compareTo(other:Object) int
   }
   class Carrier {
       -carrier_id: string
       -name: string
       -api_service: CarrierApiService
       +__init__(carrier_id, name, api_service)
       +compareTo(other:Object) int
   }
   class Rate {
       -carrier: Carrier
       -service_level: string
       -amount: float
       +__init__(carrier, service_level, amount)
       +compareTo(other:Object) int
   }
   class Label {
       -label_id: string
       -shipment_id: string
       -file_format: string
       -data: binary
       +__init__(label_id, shipment_id, file_format, data)
   }
   class Manifest {
       -manifest_id: string
       -carrier: Carrier
       -shipments: list~Shipment~
       +__init__(manifest_id, carrier, shipments)
       +add_shipment(shipment)
   }
   class CarrierApiService {
       <<abstract>>
       +get_rates(package_details): list~Rate~
       +create_label(shipment_details): Label
   }
   class FedExApiService {
       +get_rates(package_details): list~Rate~
       +create_label(shipment_details): Label
   }
   class UPSApiService {
       +get_rates(package_details): list~Rate~
       +create_label(shipment_details): Label
   }
   class USPSApiService {
       +get_rates(package_details): list~Rate~
       +create_label(shipment_details): Label
   }


   %% Inheritance (extends)
   CarrierApiService <|-- FedExApiService : extends
   CarrierApiService <|-- UPSApiService : extends
   CarrierApiService <|-- USPSApiService : extends


   Comparable <|.. Shipment : extends
   Comparable <|.. Carrier : extends
   Comparable <|.. Rate : extends


   %% Includes (uses/composes)
   ShippingService *-- Shipment : includes
   ShippingService *-- Carrier : includes
   ShippingService *-- Rate : includes
   ShippingService *-- Label : includes
   ShippingService *-- Manifest : includes


   Shipment o-- Carrier : includes
   Shipment o-- Label : includes
   Shipment "0..1" --> Manifest : includes


   Manifest o-- Carrier : includes
   Manifest *-- Shipment : includes


   Carrier o-- CarrierApiService : includes


   Rate o-- Carrier : includes
