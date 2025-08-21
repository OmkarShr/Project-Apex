# Product Catalog Service Design (Revised)

## 1. Classes and Member Methods

### Class: `ProductCatalogService`

The main service class for managing the product catalog.

| Member Method | Purpose |
| :--- | :--- |
| `create_product(product_data)` | Creates a new product in the catalog. |
| `get_product_by_sku(sku)` | Retrieves a product by its SKU. |
| `search_products(query)` | Searches for products based on a query. |
| `list_all_products()` | Lists all products in the catalog. |
| `add_attribute_to_product(sku, attribute)` | Adds an attribute to a product. |
| `get_product_attributes(sku)` | Retrieves all attributes for a product. |

### Class: `Product`

Represents a product in the catalog.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(sku, name, description)` | Initializes a new Product object. |
| `update_details(new_details)` | Updates the product's details. |
| `add_image(image)` | Adds an image to the product. |
| `get_images()` | Retrieves all images for the product. |

### Class: `SKU`

Represents a Stock Keeping Unit.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(sku_value)` | Initializes a new SKU object. |
| `validate()` | Validates the format of the SKU. |

### Class: `ProductAttribute`

A generic class for product attributes.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(name, value)` | Initializes a new ProductAttribute object. |

### Class: `Dimension`

A specific type of attribute.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(length, width, height, unit)` | Initializes a new Dimension object. |

### Class: `Weight`

A specific type of attribute.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(value, unit)` | Initializes a new Weight object. |

### Class: `HandlingRequirement`

A specific type of attribute.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(description)` | Initializes a new HandlingRequirement object. |

### Class: `StorageConstraint`

A specific type of attribute.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(description)` | Initializes a new StorageConstraint object. |

### Class: `CustomsInfo`

A specific type of attribute.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(country_of_origin, tariff_code)` | Initializes a new CustomsInfo object. |

### Class: `Image`

Represents a product image.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(image_url, alt_text)` | Initializes a new Image object. |