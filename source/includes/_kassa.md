# Kassa (legacy)

Deze endpoints worden gebruikt door de kassa/POS flows en wijken af van de REST endpoints onder `/products`, `/documents`, ...

## Products sync

### HTTPS Request

`GET https://api.onlinefact.be/kassa.php?data=products`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
from_timestamp | integer | Only return products modified after this UNIX timestamp (seconds). Use `0` or empty for initial sync.
limit | integer | Optional limit (only applied when the server detects > 10.000 changed products for the given timestamp).
version | string/decimal | Client version used for backwards compatibility rules.

### Result Parameters (per product)

Parameter | Type | Description
--------- | ------- | -----------
prodid | integer | Product id
prodref | string | Product reference
prodref2 | string | Extra reference
prodoms | string | Product description (incl. option name suffix where applicable)
prodprijsex_1 | decimal | Price excl VAT (price level 1)
prodprijsex_2 | decimal | Price excl VAT (price level 2)
prodprijsex_3 | decimal | Price excl VAT (price level 3)
prodprijsex_4 | decimal | Price excl VAT (price level 4)
prodprijsex_5 | decimal | Price excl VAT (price level 5)
prodstock | decimal | Stock
prod_tax_id | integer | Tax id
catid | integer/array | **Category id(s)**. For `version < 2.136` this is an integer. For `version >= 2.136` this is an array of unique category ids (primary category from `products.catid` + extra categories from `products_categorie`).
brand_id | integer | Brand id
barcodes | array | Barcodes
staffels | array | Price tiers
extra | array | Extra products/components

