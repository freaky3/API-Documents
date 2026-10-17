# Products


Parameter | Type | Description
--------- | ------- | -----------
product_id | integer | Unique identifier for the resource. `[read-only]`
reference | string | Unique SKU
reference2 | string | 
barcode | string | the barcode of the product, can be EAN13 of any other type
description | string | Product name
price_excl | decimal | Product price excl VAT
tax | decimal | VAT percentage of product
price_incl | decimal | Product price incl VAT 
purchaseprice_excl | decimal | Purchase price excl VAT
costprice_excl | decimal | Cost price excl VAT (Average purchase price of items in stock)
stock | decimal | amount of inventory
stock_comment | string | comment for stock change
stock_minimum | decimal | minimum stock level of the product
capacity | decimal | capacity value of the product
capacity_unit | string | unit of capacity value (KG,g,ml,L,oz,lb)
categorie_id | integer | primary category ID
brand_id | integer | brand ID
season_id | integer | linked season ID, `0` if none `[read-only]`
webshop | boolean | may this product be visible on webshop
dropship | boolean | webshop has always stock of this product
endoflife | boolean | webshop can max sell stock amount, when stock = 0 the product will go offline
free_zone | text | long description of the product (html)
binloc | string | location of the product
binloc2 | string | alternative location of the product
datemodified | date-time | date and time of last modification `[read-only]`
supplier | string | supplier of the product
HBK | decimal | amount of open client orders `[read-only]`
HBL | decimal | amount of open supplier orders `[read-only]`
managestock | boolean | does this product needs stock calculation
product_type | integer | type of product (1 = normal, 2 = textbox, 3 = option product, 4 = product with components) `[read-only]`
ledger_sale | string | sale ledger number
ledger_purchase | string | purchase ledger number
unit | string | main unit of product (ST,DOOS,BAK,KG,LITER,...)
lastsold | date | date the product is last sold `[read-only]`
categories | array | 
barcodes | array | 
images | array | 
attributes | array | Option variants (`product_type` 3). On POST this creates option products. On PUT this replaces the full option set of the product.
discount | array | 
extra | array |
component | array |

**Products - categories**

Parameter | Type | Description
--------- | ------- | -----------
categorie_id | integer | Unique identifier for the resource.

**Products - barcodes**

Parameter | Type | Description
--------- | ------- | -----------
barcode | string | the barcode of the product, can be EAN13 of any other type.

**Products - images**

Parameter | Type | Description
--------- | ------- | -----------
image_id | integer | Unique identifier for the resource. `[read-only]`
order | integer | the order of the image
datemodified | date-time | date and time of last modification `[read-only]`
image_url | string | URL of the image. `[read-only]`

**Products - languages**

Parameter | Type | Description
--------- | ------- | -----------
description | string | Product name in specified language
free_zone | text | long description of the product (html) in specified language

**Products - multi**

Multi-pack / alternative packaging definitions for a product (e.g. `PACK` of 6 units).

Parameter | Type | Description
---|---|---
unit | string | Unit of the multi-pack (e.g. `PACK`, `DOOS`, ...)
quantity | decimal | Quantity of base units in the multi-pack (e.g. `6`)
barcode | string | Barcode of the multi-pack

**Products - attributes**

On POST, include `attributes` to create a product with options (`product_type` 3). Each item becomes a variant with SKU `reference` + account option delimiter + option reference(s). On PUT, `attributes` replaces the complete option set: missing variants are removed (when not used on documents), new ones are created, existing ones are updated. A normal product (`product_type` 1) is converted to an option product when `attributes` is sent.

Parameter | Type | Description
--------- | ------- | -----------
product_id | integer | Unique identifier of the variant. `[read-only]` on GET. Optional on PUT to match an existing variant.
reference | string | Unique SKU of the variant `[read-only]`
reference2 | string | 
barcode | string | barcode of this variant
stock | decimal | inventory of this variant
addprice | decimal | price difference vs the parent `price_excl`
webshop | boolean | may this variant be visible on webshop
dropship | boolean | webshop has always stock of this variant
endoflife | boolean | webshop can max sell stock amount, when stock = 0 the variant will go offline
HBK | decimal | amount of open client orders `[read-only]`
HBL | decimal | amount of open supplier orders `[read-only]`
attribute_items | array | Required on POST/PUT. One item for a single option (e.g. size), two items for a combined option (e.g. color + size).

**Products - attributes - attribute_items**

An existing option value is reused when `attribute_id` or `attribute` (option reference) matches `options_values`. Otherwise a new option value is created.

Parameter | Type | Description
--------- | ------- | -----------
attribute_id | integer | Unique identifier of the option value. Optional on POST/PUT to select an existing option. `[read-only]` on GET.
attribute | string | Reference of the option (e.g. `L`, `BLUE`). Required on POST/PUT when `attribute_id` is not sent.
attribute_name | string | Name of the option. Used when a new option value is created; defaults to `attribute`.
attribute_group_id | integer | ID of the option group (eg: 1 = Size, 2 = Color). Used when a new option value is created.
lastsold | date | date the variant is last sold `[read-only]`


**Products - discount**

Array of discount rules applied to the product.

Parameter | Type | Description
--------- | ---- | -----------
type | integer | **Discount type**: 1 = percentage, 2 = new price (excl. VAT), 3 = price reduction (excl. VAT)
priceniveau | integer | Price level / tariff group
minimum | integer | Minimum threshold (e.g. quantity) to activate the rule
value | decimal | Value of the discount. For type 1: percentage (e.g. 5 = 5%). For type 2: new price excl. VAT. For type 3: amount excl. VAT to subtract.

**Products - extra**

Extras that are sold together with the product, each with its own VAT and quantity per main product.

Parameter | Type | Description
--------- | ---- | -----------
product_id | integer | Product ID of the extra item
reference | string | SKU/reference of the extra item
price_excl | decimal | Price excl. VAT per unit of the extra `[read-only]`
tax | decimal | VAT percentage for the extra `[read-only]`
quantity | integer | Quantity of the extra per main product

**Products - component**

Components are underlaying products that are part of the main product. eg: gift basket

Parameter | Type | Description
--------- | ---- | -----------
product_id | integer | Product ID of the component item
reference | string | SKU/reference of the component item
quantity | integer | Quantity of the component per main product

## Get All Products

```php

$ch = curl_init('https://api.onlinefact.be/products/');
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, "api_key_here:api_secret_here");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/products" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
  {
    "product_id": "8",
    "reference": "MENTOS",
    "reference2": "",
    "barcode": "80723028",
    "description": "Mentos White",
    "price_excl": "3.301900",
    "tax": "6.0",
    "price_incl": "3.50",
    "stock": "-27.00",
    "stock_minimum": "5.00",
    "capacity": "0.280",
    "capacity_unit": "KG",
    "categorie_id": "155",
    "brand_id": "0",
    "webshop": "0",
    "dropship": "0",
    "endoflife": "0",
    "free_zone": "",
    "binloc": "",
    "binloc2": "",
    "datemodified": "2022-10-25 11:47:14",
    "supplier": "",
    "HBK": "0",
    "HBL": "21",
    "managestock": "1",
    "product_type": "1",
    "ledger_sale": "700000",
    "ledger_purchase": "600000",
    "unit": "ST",
    "lastsold": "20-10-2022",
    "categories": [
      "155"
    ],
    "barcodes": [
      "80723028"
    ],
    "multi": [
      { "unit": "PACK", "quantity": 6, "barcode": "1451902048154" }
    ],
    "languages": {
      "NL": {
        "description": "Mentos White",
        "free_zone": ""
      },
      "FR": {
        "description": "Mentos Blanc",
        "free_zone": ""
      }
    },
    "images": [
      {
        "image_id": "2",
        "order": "1",
        "datemodified": "2022-10-25 11:47:08",
        "image_url": "https://admin.onlinefact.be/image.php?image=cHJvZHVjdGltYWdlfDE1fDh8Mg=="
      },
      {
        "image_id": "1",
        "order": "2",
        "datemodified": "2022-10-25 11:46:54",
        "image_url": "https://admin.onlinefact.be/image.php?image=cHJvZHVjdGltYWdlfDE1fDh8MQ=="
      }
    ],
    "discount": [
      {
        "type": 1,
        "priceniveau": 1,
        "minimum": 10,
        "value": 5
      },
      {
        "type": 2,
        "priceniveau": 1,
        "minimum": 24,
        "value": 150
      },
      {
        "type": 3,
        "priceniveau": 3,
        "minimum": 6,
        "value": 0.5
      }
    ],
    "extra": [
      {
        "product_id": 70,
        "reference": "P70",
        "price_excl": 0.25,
        "tax": 0,
        "quantity": 1
      }
    ]
  },
  {
    "product_id": "8105",
    "reference": "P8105",
    "reference2": "",
    "barcode": "1501328314675",
    "description": "Bloes Zomerprint",
    "price_excl": "24.752100",
    "tax": "21.0",
    "price_incl": "29.95",
    "stock": "-1.00",
    "stock_minimum": "0.00",
    "capacity": "0.000",
    "capacity_unit": "",
    "categorie_id": "0",
    "brand_id": "0",
    "webshop": "0",
    "dropship": "0",
    "endoflife": "0",
    "free_zone": "",
    "binloc": "",
    "binloc2": "",
    "datemodified": "2022-10-11 10:06:21",
    "supplier": "",
    "HBK": "0",
    "HBL": "0",
    "prodstocka": "1",
    "product_type": "3",
    "ledger_sale": "700000",
    "ledger_purchase": "600000",
    "unit": "ST",
    "categories": [],
    "images": [],
    "languages": {
      "NL": {
        "description": "Bloes Zomerprint",
        "free_zone": ""
      },
      "FR": {
        "description": "",
        "free_zone": ""
      }
    },
    "attributes": [
      {
        "product_id": "8107",
        "reference": "P8105.L",
        "reference2": "",
        "barcode": "1463808308362",
        "stock": "-1.00",
        "addprice": 0,
        "webshop": "0",
        "dropship": "0",
        "endoflife": "0",
        "hbk": "0",
        "hbl": "0",
        "attribute_items": [
          {
            "attribute_id": "5",
            "attribute": "L",
            "attribute_name": "Large",
            "attribute_group_id": "1",
            "lastsold": "11-10-2022"
          }
        ]
      },
      {
        "product_id": "8106",
        "reference": "P8105.M",
        "reference2": "",
        "barcode": "1535955444924",
        "stock": "-4.00",
        "addprice": 0,
        "webshop": "0",
        "dropship": "0",
        "endoflife": "0",
        "hbk": "5",
        "hbl": "0",
        "attribute_items": [
          {
            "attribute_id": "4",
            "attribute": "M",
            "attribute_name": "Medium",
            "attribute_group_id": "1",
            "lastsold": "11-10-2022"
          }
        ]
      },
      {
        "product_id": "8105",
        "reference": "P8105.S",
        "reference2": "",
        "barcode": "1501328314675",
        "stock": "-1.00",
        "addprice": 0,
        "webshop": "0",
        "dropship": "0",
        "endoflife": "0",
        "hbk": "0",
        "hbl": "0",
        "attribute_items": [
          {
            "attribute_id": "3",
            "attribute": "S",
            "attribute_name": "Small ",
            "attribute_group_id": "1",
            "lastsold": "11-10-2022"
          }
        ]
      }
    ],
    "discount": [],
    "extra": []
  }
]
```

This endpoint retrieves all products.

### HTTPS Request

`GET https://api.onlinefact.be/products/?PARAMETER=VALUE`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000).
page | 1 | page number of result.
from_id || products added sinds this product_id
from_datemodified || products changed sinds this timestamp
description || part of description
reference || 
reference2 ||
categorie_id || 
brand_id ||
webshop || 1 = visible on webshop, 0 = NOT visible on webshop
dropship || 1 = dropshop ON , 0 = dropshop OFF
binloc || name of stock location
barcode || 


## Get a Specific Product

```php

$ch = curl_init('https://api.onlinefact.be/products/8/');
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, "api_key_here:api_secret_here");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/products/8/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
  "product_id": "8",
  "reference": "MENTOS",
  "reference2": "",
  "barcode": "80723028",
  "description": "Mentos White",
  "price_excl": "3.301900",
  "tax": "6.0",
  "price_incl": "3.50",
  "stock": "-27.00",
  "stock_minimum": "5.00",
  "capacity": "0.280",
  "capacity_unit": "KG",
  "categorie_id": "155",
  "brand_id": "0",
  "webshop": "0",
  "dropship": "0",
  "endoflife": "0",
  "free_zone": "",
  "binloc": "",
  "binloc2": "",
  "datemodified": "2022-10-25 11:47:14",
  "supplier": "",
  "HBK": "0",
  "HBL": "21",
  "managestock": "1",
  "product_type": "1",
  "ledger_sale": "700000",
  "ledger_purchase": "600000",
  "unit": "ST",
  "lastsold": "20-10-2022",
  "categories": [
    "155"
  ],
  "multi": [
    { "unit": "PACK", "quantity": 6, "barcode": "1451902048154" }
  ],
  "barcodes": [
    "80723028"
  ],
  "languages": {
    "NL": {
      "description": "Mentos White",
      "free_zone": ""
    },
    "FR": {
      "description": "Mentos Blanc",
      "free_zone": ""
    }
  },
  "images": [
    {
      "image_id": "2",
      "order": "1",
      "datemodified": "2022-10-25 11:47:08",
      "image_url": "https://admin.onlinefact.be/image.php?image=cHJvZHVjdGltYWdlfDE1fDh8Mg=="
    },
    {
      "image_id": "1",
      "order": "2",
      "datemodified": "2022-10-25 11:46:54",
      "image_url": "https://admin.onlinefact.be/image.php?image=cHJvZHVjdGltYWdlfDE1fDh8MQ=="
    }
  ],
  "discount": [
    {
      "type": 1,
      "priceniveau": 1,
      "minimum": 10,
      "value": 5
    }
  ],
  "extra": [
    {
      "product_id": 70,
      "reference": "P70",
      "price_excl": 0.25,
      "tax": 0,
      "quantity": 1
    }
  ]
}
```

This endpoint retrieves a specific product.

### HTTPS Request

`GET https://api.onlinefact.be/products/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the product to retrieve

## Add a Product

```php

$data_string = '{"reference":"MENTOS",
                "barcode":"80723028",
                "description":"Mentos White 25pack",
                "tax":"6",
                "price_incl":"3.50",
                "categorie_id":"155",
                "supplier":"Carrefour",
                "managestock":"1",
                "attributes":[
                  {
                    "barcode":"80723029",
                    "stock":"10",
                    "addprice":"0",
                    "attribute_items":[
                      {"attribute":"S","attribute_name":"Small","attribute_group_id":1}
                    ]
                  },
                  {
                    "barcode":"80723030",
                    "stock":"8",
                    "addprice":"0.20",
                    "attribute_items":[
                      {"attribute":"M","attribute_name":"Medium","attribute_group_id":1}
                    ]
                  }
                ]
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/products/");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, "api_key_here:api_secret_here");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/products/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{"reference":"MENTOS",
      "barcode":"80723028",
      "description":"Mentos White 25pack",
      "tax":"6",
      "price_incl":"3.50",
      "categorie_id":"155",
      "supplier":"Carrefour",
      "managestock":"1",
      "attributes":[
        {
          "barcode":"80723029",
          "stock":"10",
          "addprice":"0",
          "attribute_items":[
            {"attribute":"S","attribute_name":"Small","attribute_group_id":1}
          ]
        },
        {
          "barcode":"80723030",
          "stock":"8",
          "addprice":"0.20",
          "attribute_items":[
            {"attribute":"M","attribute_name":"Medium","attribute_group_id":1}
          ]
        }
      ]
      }'
```

> The above command returns JSON structured like this:

```json
{
  "product_id": "8",
  "reference": "MENTOS",
  "reference2": "",
  "barcode": "80723028",
  "description": "Mentos White 25pack",
  "price_excl": "3.301900",
  "tax": "6.0",
  "price_incl": "3.50",
  "stock": "0",
  "stock_minimum": "0.00",
  "capacity": "0.280",
  "capacity_unit": "KG",
  "categorie_id": "155",
  "brand_id": "0",
  "webshop": "0",
  "dropship": "0",
  "endoflife": "0",
  "free_zone": "",
  "binloc": "",
  "binloc2": "",
  "datemodified": "2022-10-25 11:47:14",
  "supplier": "Carrefour",
  "HBK": "0",
  "HBL": "0",
  "managestock": "1",
  "product_type": "1",
  "ledger_sale": "700000",
  "ledger_purchase": "600000",
  "unit": "ST",
  "multi": [],
  "categories": [],
  "barcodes": [],
  "images": []
}
```

This endpoint adds a product. Send `attributes` to create option variants (`product_type` 3) in the same request. Without `attributes` a normal product (`product_type` 1) is created.

### HTTPS Request

`POST https://api.onlinefact.be/products/`

### URL Parameters

Parameter | Description
--------- | -----------
reference | The unique SKU of the product. For option products this is the base SKU; each variant becomes `reference` + delimiter + option reference.


## Update a Product

```php

$data_string = '{
                 "description":"Mentos White 25pack",
                 "supplier":"Carrefour",
                 "attributes":[
                   {
                     "barcode":"80723029",
                     "stock":"10",
                     "attribute_items":[
                       {"attribute":"S","attribute_name":"Small","attribute_group_id":1}
                     ]
                   },
                   {
                     "barcode":"80723030",
                     "stock":"8",
                     "addprice":"0.20",
                     "attribute_items":[
                       {"attribute":"M","attribute_name":"Medium","attribute_group_id":1}
                     ]
                   }
                 ]
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/products/8/");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, "api_key_here:api_secret_here");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/products/8/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "description":"Mentos White 25pack",
        "supplier":"Carrefour",
        "attributes":[
          {
            "barcode":"80723029",
            "stock":"10",
            "attribute_items":[
              {"attribute":"S","attribute_name":"Small","attribute_group_id":1}
            ]
          },
          {
            "barcode":"80723030",
            "stock":"8",
            "addprice":"0.20",
            "attribute_items":[
              {"attribute":"M","attribute_name":"Medium","attribute_group_id":1}
            ]
          }
        ]
      }'
```

> The above command returns JSON structured like this:

```json
{
  "product_id": "8",
  "reference": "MENTOS",
  "reference2": "",
  "barcode": "80723028",
  "description": "Mentos White 25pack",
  "price_excl": "3.301900",
  "tax": "6.0",
  "price_incl": "3.50",
  "stock": "-27.00",
  "stock_minimum": "5.00",
  "capacity": "0.280",
  "capacity_unit": "KG",
  "categorie_id": "155",
  "brand_id": "0",
  "webshop": "0",
  "dropship": "0",
  "endoflife": "0",
  "free_zone": "",
  "binloc": "",
  "binloc2": "",
  "datemodified": "2022-10-25 11:47:14",
  "supplier": "Carrefour",
  "HBK": "0",
  "HBL": "21",
  "managestock": "1",
  "product_type": "1",
  "ledger_sale": "700000",
  "ledger_purchase": "600000",
  "unit": "ST",
  "multi": [],
  "lastsold": "20-10-2022",
  "categories": [
    "155"
  ],
  "barcodes": [
    "80723028"
  ],
  "images": []
}
```

This endpoint updates a specific product. Send `attributes` to add or replace option variants. A normal product is converted to `product_type` 3. The array is the full option set: variants that are omitted are deleted when they are not used on documents.

### HTTPS Request

`PUT https://api.onlinefact.be/products/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the product to update


## Delete a product

```php
$ch = curl_init("https://api.onlinefact.be/products/8/");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "DELETE");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, "api_key_here:api_secret_here");
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/products/8/" \
  -X DELETE \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```


> The above command returns JSON structured like this:

```json
{
  "product_id": "8",
  "deleted": "success"
}
```

This endpoint deletes a specific product.

### HTTPS Request

`DELETE https://api.onlinefact.be/products/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the product to delete