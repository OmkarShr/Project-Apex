# Product Catalog Service Design (Revised)

## 1. Classes and Member Methods

### Class: ProductCatalogService

| Member Method              | Purpose                                   |
|----------------------------|-------------------------------------------|
| create_product(product_data)  | Creates a new product in the catalog.    |
| get_product_by_sku(sku)      | Retrieves a product by its SKU.           |
| search_products(query)       | Searches for products based on a query.  |
| list_all_products()          | Lists all products in the catalog.        |
| add_attribute_to_product(sku, attribute) | Adds an attribute to a product.       |
| get_product_attributes(sku)  | Retrieves all attributes for a product.   |

---

### Class: Product

| Member Method     | Purpose                          |
|-------------------|----------------------------------|
| get_sku()         | Returns the SKU of the product.  |
| get_type()        | Returns the product type.         |
| get_stock_quantity() | Returns available stock quantity. |
| update_stock_quantity(quantity) | Updates stock quantity.       |

---

### Class: SKU

| Member Method       | Purpose                        |
|---------------------|--------------------------------|
| get_value()         | Returns the stored SKU value.   |
| validate()          | Validates the SKU format.       |

---

### Class: ProductType

| Member Method       | Purpose                                 |
|---------------------|-----------------------------------------|
| get_category()      | Returns the product category.            |
| get_attributes()    | Returns attributes related to the type. |
| add_attribute(attr) | Adds an attribute to the product type.  |
