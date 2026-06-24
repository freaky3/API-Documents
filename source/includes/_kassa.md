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

## Customers sync

### HTTPS Request

`GET https://api.onlinefact.be/kassa.php?data=customers`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
from_timestamp | integer | Only return customers modified after this UNIX timestamp (seconds). Use `0` or empty for initial sync.
limit | integer | Optional limit on the number of returned customers.
version | string/decimal | Client version used for backwards compatibility rules.

### Result Parameters (per customer)

Parameter | Type | Description
--------- | ------- | -----------
custid | integer | Customer id
custref | string | Customer reference
custname1 | string | Name or company
custbarcode | string | Barcode of the loyalty card
custexcludeloyalty | string | `"1"` = customer is excluded from earning loyalty points/credit (existing balance can still be redeemed); `"0"` = normal (default)
points | integer | Current loyalty points balance
invoicemail | string | `"1"` = send invoices by email
datemodified | integer | UNIX timestamp of last modification

<aside class="notice">Only the loyalty-specific field is documented here in full; the customers sync returns the same address/contact fields as the rest of the customer model.</aside>

### Up-sync (POST)

`POST https://api.onlinefact.be/kassa.php`

The POS can push customer changes back to the server. Each customer object in the payload may include `custexcludeloyalty` (`0`|`1`). When the field is omitted (older POS versions), the server keeps the value at `0`. When `custexcludeloyalty = 1`, the server skips earning new loyalty points/credit for that customer's tickets, while redeeming an existing balance stays possible.

