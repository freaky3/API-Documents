# Documents

Parameter | Type | Description
--------- | ------- | -----------
document_id | integer | Unique identifier for the resource. `[read-only]`
name | string | Name of documenttype.
document_type | integer | 1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket.
number | string | document number `[read-only]`
document_date | data | date of document
expiration_date | data | date of document
reference | string | reference of document
comments | string | comments on the document
checkoutnumber | integer | number of checkout where the document is made
paymentmsg | string | prefered message for payment `[read-only]`
closed | integer | is document closed `[read-only]`
currency | string | currency used `[read-only]`
total_vatexcl | decimal | document total excl TAX `[read-only]`
total_vatincl | decimal | document total incl TAX `[read-only]`
total_vat | decimal | TAX amount `[read-only]`
total_vatincl_beforerounding | decimal | total before rounding to 5c (if applied) `[read-only]`
payd | decimal | amount already payd `[read-only]`
saldo | decimal | open amount that is not payd yet `[read-only]`
cash_discount | decimal | percentage of cash discount
cash_discount_amount | decimal | amount of cash discount that can be applied `[read-only]`
ogm | integer | unique document number used to do payments `[read-only]`
user_created_name | string | name of Onlinefact user who created this document `[read-only]`
user_created_id | integer | ID of Onlinefact user wo created this document `[read-only]`
datemodified | datetime | timestamp of last modification `[read-only]`
url_pdf | string | URL of PDF document `[read-only]`
customer | array |
delivery | array |
lines | array |
tax | array |
payments | array |
customfields | array |

**Documents - customers**

Parameter | Type | Description
--------- | ------- | -----------
customer_id | integer | Unique identifier for the resource. `[read-only]`
reference | string | Unique code.
name | string | Name or company.
name2 | string | 
address | string | Address of customer
address2 | string | 
zip | string | Postal code for invoice
city | string | City for invoice
phone | string | Landline phone number
mobile | string | Mobile phone number
taxnr | string | National TAX number.
email | string | Default email address of customer
country | string | country name
country_id | integer | <a href="https://en.wikipedia.org/wiki/ISO_3166-1_numeric" target="_blank">ISO 3166-1</a> numeric code of the country 

**Documents - delivery**

Parameter | Type | Description
--------- | ------- | -----------
name | string | Delivery address name
address | string | Delivery address
zip | string | Delivery address zip
city | string | Delivery address city
country | string | Delivery address country name

**Documents - lines**

Parameter | Type | Description
--------- | ------- | -----------
line | integer | line number `[read-only]`
product_id | integer | Unique identifier of the product. `[read-only]`
reference | string | reference of the product.
reference2 | string | reference of the product.
product_text | string | text if line is textbox
description | string | text or productname on line
costprice | decimal | cost price on this line
price_vatexcl | decimal | unit price excl TAX
price_vatincl | decimal | unit price incl TAX
quantity | decimal | quantity
delivered | decimal | quantity that is delivered but not invoiced yet (only document_type = 2)
delivered_already | decimal | quantity that is delivered and invoiced (only document_type = 2) `[read-only]`
unit | string | unit name
unit_multipier | decimal | multiply amount for unit *(total qty = quantity x unit_multipier)*
parent | decimal | if product is component of other product, this is the line number of main product
tax | decimal | tax of this line
discount | decimal | discount percentage
subtotal_vat | decimal | TAX subtotal of line
subtotal_vatexcl | decimal | subtotal of line excl TAX
subtotal_vatincl | decimal | subtotal of line incl TAX

**Documents - tax**

Parameter | Type | Description
--------- | ------- | -----------
tax_pct | decimal | Tax Percentage
tax_excl | decimal | Total excl TAX of this Tax Percentage
tax | decimal | Total TAX of this Tax Percentage
tax_incl | decimal | Total incl TAX of this Tax Percentage

**Documents - payments**

Parameter | Type | Description
--------- | ------- | -----------
type | integer | ID of payment method
method | string | Name of payment method
amount | decimal | Payment amount
date | date | Date of this payment

**Documents - customfields**

Parameter | Type | Description
--------- | ------- | -----------
{name of field} | string | value of field


## Get All Documents

```php

$ch = curl_init('https://api.onlinefact.be/documents/');
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
curl "https://api.onlinefact.be/documents/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "document_id":"468",
      "name":"Bestelling",
      "document_type":"2",
      "number":"2022\/26",
      "document_date":"2022-11-07",
      "expiration_date":"2022-11-07",
      "reference":"",
      "comments":"\n",
      "checkoutnumber":"1",
      "paymentmsg":"+++000\/7667\/72771+++",
      "closed":0,
      "currency":"EUR",
      "total_vatexcl":93.4000,
      "total_vat":5.60,
      "total_vatincl":99,
      "total_vatincl_beforerounding":0,
      "payd":null,
      "saldo":99,
      "cash_discount":"0.00",
      "cash_discount_amount":0,
      "ogm":"000766772771",
      "user_created_name":"Kristof",
      "user_created_id":"1",
      "datemodified": "2022-11-07 11:18:44",
      "url_pdf":"https:\/\/admin.onlinefact.be\/docview\/cXM5eWtmKzg5ZXlPWm9uUmpFRkphdz09",
      "customer":{
         "customer_id":"186",
         "reference":"118464",
         "name":"Vanessa Cristiano",
         "name2":"",
         "address":"Test",
         "address2":"",
         "zip":"3582",
         "city":"Koersel",
         "phone":"",
         "mobile":"",
         "taxnr":"",
         "email":"vanessa@onlinefact.be",
         "country":"Belgium",
         "country_id":"56"
      },
      "delivery":{
         "name":"",
         "address":"",
         "zip":"",
         "city":"",
         "country":""
      },
      "lines":[
         {
            "line":"1",
            "product_id":"8034",
            "reference":"P8034",
            "reference2":"",
            "product_text":"",
            "description":"Boomschors",
            "costprice":"0.000000",
            "price_vatexcl":93.3962,
            "price_vatincl":99,
            "quantity":1,
            "unit":"m³",
            "unit_multiplier":"1.000000",
            "parent":"0",
            "tax":6,
            "discount":0,
            "subtotal_vat":5.60,
            "subtotal_vatexcl":93.4000,
            "subtotal_vatincl":99
         },
         {
            "line":"2",
            "product_id":"0",
            "description":null,
            "costprice":"0.000000",
            "price_vatexcl":0,
            "price_vatincl":0,
            "quantity":1,
            "unit":null,
            "unit_multiplier":"1.000000",
            "parent":"0",
            "tax":6,
            "discount":0,
            "subtotal_vat":0,
            "subtotal_vatexcl":0,
            "subtotal_vatincl":0
         }
      ],
      "tax":[
         {
            "tax_pct":"6.0",
            "tax_excl":93.4000,
            "tax":5.60,
            "tax_incl":99
         }
      ],
      "payments":[
         
      ],
      "customfields":[
      ]
   },
   {
      "document_id":"467",
      "name":"Factuur",
      "document_type":"3",
      "number":"2022\/83",
      "document_date":"2022-11-07",
      "expiration_date":"2022-11-21",
      "reference":null,
      "comments":"",
      "checkoutnumber":"0",
      "paymentmsg":"+++000\/7651\/34380+++",
      "closed":0,
      "currency":"EUR",
      "total_vatexcl":29.6600,
      "total_vat":4.94,
      "total_vatincl":34.60,
      "total_vatincl_beforerounding":0,
      "payd":34.60,
      "saldo":0,
      "cash_discount":"0.00",
      "cash_discount_amount":0,
      "ogm":"000765134380",
      "user_created_name":"Kristof",
      "user_created_id":"1",
      "datemodified": "2022-11-07 11:18:44",
      "url_pdf":"https:\/\/admin.onlinefact.be\/docview\/OCs3Ny93VWp0R0JHWEx3SWZkK1lJdz09",
      "customer":{
         "customer_id":"23",
         "reference":"DATACON",
         "name":"BVBA DATACON 6",
         "name2":"",
         "address":"Schootstraat 191",
         "address2":"",
         "zip":"3550",
         "city":"Heusden-Zolder",
         "phone":"+3211743319",
         "mobile":"",
         "taxnr":"BE0894069190",
         "email":"info@datacon-bvba.be",
         "country":"Belgium",
         "country_id":"56"
      },
      "delivery":{
         "name":"",
         "address":"",
         "zip":"",
         "city":"",
         "country":""
      },
      "lines":[
         {
            "line":"1",
            "product_id":"8",
            "reference":"MENTOS",
            "reference2":"",
            "product_text":"",
            "description":"Mentos White",
            "costprice":"1.500000",
            "price_vatexcl":2.0283,
            "price_vatincl":2.15,
            "quantity":5,
            "unit":"ST",
            "unit_multiplier":"1.000000",
            "parent":"0",
            "tax":6,
            "discount":0,
            "discount":15,
            "subtotal_vat":0.52,
            "subtotal_vatexcl":8.6200,
            "subtotal_vatincl":9.14
         },
         {
            "line":"2",
            "product_id":"8106",
            "reference":"P8105.M",
            "reference2":"",
            "product_text":"",
            "description":"Bloes Zomerprint Medium",
            "costprice":"0.000000",
            "price_vatexcl":24.7521,
            "price_vatincl":29.95,
            "quantity":1,
            "unit":"",
            "unit_multiplier":"1.000000",
            "parent":"0",
            "tax":21,
            "discount":0,
            "discount":15,
            "subtotal_vat":4.42,
            "subtotal_vatexcl":21.0400,
            "subtotal_vatincl":25.46
         },
         {
            "line":"3",
            "product_id":"0",
            "description":null,
            "costprice":"0.000000",
            "price_vatexcl":0,
            "price_vatincl":0,
            "quantity":1,
            "unit":null,
            "unit_multiplier":"1.000000",
            "parent":"0",
            "tax":21,
            "discount":0,
            "subtotal_vat":0,
            "subtotal_vatexcl":0,
            "subtotal_vatincl":0
         }
      ],
      "tax":[
         {
            "tax_pct":"6.0",
            "tax_excl":8.62,
            "tax":0.52,
            "tax_incl":9.14
         },
         {
            "tax_pct":"21.0",
            "tax_excl":21.04,
            "tax":4.42,
            "tax_incl":25.46
         }
      ],
      "payments":[
         {
            "type":"2",
            "method":"Cash",
            "amount":"34.60",
            "date":"2022-11-07"
         }
      ],
      "customfields":[
      ]
   }
]
```

This endpoint retrieves all documents.

### HTTPS Request

`GET https://api.onlinefact.be/documents/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
min_date | date | Return documents for a specific start date, the date need to be in the YYYY-MM-DD format `[optional]`
max_date | date | Return documents for a specific end date, the date need to be in the YYYY-MM-DD format `[optional]`
document_type | integer |1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket
from_document_id | integer | Return documents from this document_id
from_datemodified | integer | Return documents from this timestamp `unix timestamp format`
filter | string | Filter result with string data *(customer reference/name)*
closed | integer | 0 = get only open documents
limit | 10 | limit results (max 100).
page | 1 | page number of result.


## Get a Specific Document

```php

$ch = curl_init('https://api.onlinefact.be/documents/468/');
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
curl "https://api.onlinefact.be/documents/468/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
    "document_id":"468",
    "name":"Bestelling",
    "document_type":"2",
    "number":"2022\/26",
    "document_date":"2022-11-07",
    "expiration_date":"2022-11-07",
    "reference":"",
    "comments":"\n",
    "checkoutnumber":"1",
    "paymentmsg":"+++000\/7667\/72771+++",
    "closed":0,
    "currency":"EUR",
    "total_vatexcl":93.4000,
    "total_vat":5.60,
    "total_vatincl":99,
    "total_vatincl_beforerounding":0,
    "payd":null,
    "saldo":99,
    "cash_discount":"0.00",
    "cash_discount_amount":0,
    "ogm":"000766772771",
    "user_created_name":"Kristof",
    "user_created_id":"1",
    "datemodified": "2022-11-07 11:18:44",
    "url_pdf":"https:\/\/admin.onlinefact.be\/docview\/cXM5eWtmKzg5ZXlPWm9uUmpFRkphdz09",
    "customer":{
        "customer_id":"186",
        "reference":"118464",
        "name":"Vanessa Cristiano",
        "name2":"",
        "address":"Test",
        "address2":"",
        "zip":"3582",
        "city":"Koersel",
        "phone":"",
        "mobile":"",
        "taxnr":"",
        "email":"vanessa@onlinefact.be",
        "country":"Belgium",
        "country_id":"56"
    },
    "delivery":{
        "name":"",
        "address":"",
        "zip":"",
        "city":"",
        "country":""
    },
    "lines":[
        {
        "line":"1",
        "product_id":"8034",
        "reference":"P8034",
        "reference2":"",
        "product_text":"",
        "description":"Boomschors",
        "costprice":"0.000000",
        "price_vatexcl":93.3962,
        "price_vatincl":99,
        "quantity":1,
        "unit":"m³",
        "unit_multiplier":"1.000000",
        "parent":"0",
        "tax":6,
        "discount":0,
        "subtotal_vat":5.60,
        "subtotal_vatexcl":93.4000,
        "subtotal_vatincl":99
        },
        {
        "line":"2",
        "product_id":"0",
        "description":null,
        "costprice":"0.000000",
        "price_vatexcl":0,
        "price_vatincl":0,
        "quantity":1,
        "unit":null,
        "unit_multiplier":"1.000000",
        "parent":"0",
        "tax":6,
        "discount":0,
        "subtotal_vat":0,
        "subtotal_vatexcl":0,
        "subtotal_vatincl":0
        }
    ],
    "tax":[
        {
        "tax_pct":"6.0",
        "tax_excl":93.40,
        "tax":5.60,
        "tax_incl":99
        }
    ],
    "payments":[
        
    ],
    "customfields":[
    ]
}
```

This endpoint retrieves a specific document.

### HTTPS Request

`GET https://api.onlinefact.be/documents/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the document to retrieve

## Add a Document

```php


$data_string = '{
                  "document_type":3,
                  "document_date":"2022-11-07",
                  "expiration_date":"2022-11-07",
                  "document_time":"12:35",
                  "reference":"",
                  "sendwithpeppol":1,
                  "customer_id":23,
                  "payment_method":8,
                  "lines":[
                     {
                     "reference":"MENTOS",
                     "quantity":1,
                     "description":"Menthos White 25 pack",
                     "price_vatexcl":5.6132,
                     "tax":6,
                     "discount":0
                     },
                     {
                     "reference":"LAYSPAP30",
                     "quantity":2,
                     "description":"Lays paprika 30 g",
                     "price_vatexcl":1.6509,
                     "tax":6,
                     "discount":0
                     }
                  ]
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/documents/");
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
curl "https://api.onlinefact.be/documents/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "document_type":3,
        "document_date":"2022-11-07",
        "expiration_date":"2022-11-07",
        "document_time":"12:35",
        "reference":"",
        "customer_id":23,
        "payment_method":8,
        "lines":[
            {
            "reference":"MENTOS",
            "quantity":1,
            "description":"Menthos White 25 pack",
            "price_vatexcl":5.6132,
            "tax":6,
            "discount":0
            },
            {
            "reference":"LAYSPAP30",
            "quantity":2,
            "description":"Lays paprika 30 g",
            "price_vatexcl":1.6509,
            "tax":6,
            "discount":0
            }
        ]
    }'
```

> The above command returns JSON structured like this:

```json
{
   "document_id":"475",
   "name":"Bestelling",
   "document_type":"3",
   "number":"2022\/29",
   "document_date":"2022-11-07",
   "expiration_date":"2022-11-07",
   "reference":"",
   "comments":"",
   "checkoutnumber":"0",
   "paymentmsg":"+++000\/7782\/41508+++",
   "closed":0,
   "currency":"EUR",
   "total_vatexcl":8.92,
   "total_vat":0.53,
   "total_vatincl":9.45,
   "total_vatincl_beforerounding":0,
   "payd":9.45,
   "saldo":0,
   "cash_discount":"0.00",
   "cash_discount_amount":0,
   "ogm":"000778241508",
   "user_created_name":"",
   "user_created_id":"",
   "datemodified": "2022-11-07 11:18:44",
   "url_pdf":"https:\/\/admin.onlinefact.be\/docview\/MDg2RnVWZHVzVSt0Y2NNSE50S3lMdz09",
   "customer":{
      "customer_id":"23",
      "reference":"DATACON",
      "name":"BVBA DATACON",
      "name2":"",
      "address":"Schootstraat 191",
      "address2":"",
      "zip":"3550",
      "city":"Heusden-Zolder",
      "phone":"+3211743319",
      "mobile":"",
      "taxnr":"BE0894069190",
      "email":"info@datacon-bvba.be",
      "country":"Belgium",
      "country_id":"56"
   },
   "delivery":{
      "name":"",
      "address":"",
      "zip":"",
      "city":"",
      "country":""
   },
   "lines":[
      {
         "line":"1",
         "product_id":"8",
         "reference":"MENTOS",
         "reference2":"",
         "product_text":"",
         "description":"Menthos White 25 pack",
         "costprice":"1.500000",
         "price_vatexcl":5.6132,
         "price_vatincl":5.95,
         "quantity":1,
         "delivered":0,
         "alreadydelivered":0,
         "unit":"ST",
         "unit_multiplier":"1.000000",
         "parent":"0",
         "tax":6,
         "discount":0,
         "subtotal_vat":0.34,
         "subtotal_vatexcl":5.61,
         "subtotal_vatincl":5.95
      },
      {
         "line":"2",
         "product_id":"7675",
         "reference":"LAYSPAP30",
         "reference2":"",
         "product_text":"",
         "description":"Lays paprika 30 g",
         "costprice":"0.500000",
         "price_vatexcl":1.6509,
         "price_vatincl":1.75,
         "quantity":2,
         "delivered":0,
         "alreadydelivered":0,
         "unit":"",
         "unit_multiplier":"1.000000",
         "parent":"0",
         "tax":6,
         "discount":0,
         "subtotal_vat":0.20,
         "subtotal_vatexcl":3.30,
         "subtotal_vatincl":3.5
      }
   ],
   "tax":[
      {
         "tax_pct":"6.0",
         "tax_excl":8.92,
         "tax":0.53,
         "tax_incl":9.45
      }
   ],
   "payments":[
      {
         "type":"8",
         "method":"Webshop",
         "amount":"9.45",
         "date":"2022-11-07"
      }
   ],
   "customfields":null
}
```

This endpoint add a document.

### HTTPS Request

`POST https://api.onlinefact.be/documents/`

### URL Parameters

Parameter | Description
--------- | ------- 
document_date | Document date
document_time | Document time
document_type | 1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket, 99 = Kassa parking
reference | reference of document
sendwithpeppol| 1 = submit the document with peppol to customer (only document_type 3 and 4)
customer_id | ID of customer for this document
payment_method | type of payment used (1 = not payd, 2 = cash, 3 = banktransfer, 4 = creditcard, 5 = cheque, 6 = bancontact, 8 = webshop, 11 = paypal, 99 = voucher)
lines | array 

**Documents - lines**

Parameter | Description
--------- | -----------
reference | reference of the product to find it in the database *(product_id can also be used)*
description | text or productname
quantity | quantity
price_vatexcl | unit price excl TAX
tax | tax of this line
discount | discount on this line
delivered | number of items picked *(only for document_type = 2)*

## Update a Document

```php

$data_string = '{
                  "expiration_date":"2022-11-10",
                  "reference":"ABCD123"
                  "lines":[
                     {
                        "line":"1",
                        "description":"MENTOS met een smaakje",
                        "quantity":3,
                     },
                     {
                        "line":"2",
                        "delivered":1,
                     }
                  ]
               }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/documents/475/");
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
curl "https://api.onlinefact.be/documents/475/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
      "expiration_date":"2022-11-10",
      "reference":"ABCD123"
      "lines":[
         {
            "line":"1",
            "description":"MENTOS met een smaakje",
            "quantity":3,
         },
         {
            "line":"2",
            "picked":1,
         }
      ]
   }'
```

> The above command returns JSON structured like this:

```json
{
   "document_id":"475",
   "name":"Bestelling",
   "document_type":"2",
   "number":"2022\/29",
   "document_date":"2022-11-07",                //Update possible
   "expiration_date":"2022-11-10",              //Update possible
   "reference":"ABCD123",                       //Update possible
   "comments":"",
   "checkoutnumber":"0",
   "paymentmsg":"+++000\/7782\/41508+++",
   "closed":0,
   "currency":"EUR",
   "total_vatexcl":8.92,
   "total_vat":0.53,
   "total_vatincl":9.45,
   "total_vatincl_beforerounding":0,
   "payd":9.45,
   "saldo":0,
   "cash_discount":"0.00",
   "cash_discount_amount":0,
   "ogm":"000778241508",
   "user_created_name":"",
   "user_created_id":"",
   "datemodified": "2022-11-07 11:18:44",
   "url_pdf":"https:\/\/admin.onlinefact.be\/docview\/MDg2RnVWZHVzVSt0Y2NNSE50S3lMdz09",
   "customer":{
      "customer_id":"23",                       //Update possible
      "reference":"DATACON",
      "name":"BVBA DATACON",
      "name2":"",
      "address":"Schootstraat 191",
      "address2":"",
      "zip":"3550",
      "city":"Heusden-Zolder",
      "phone":"+3211743319",
      "mobile":"",
      "taxnr":"BE0894069190",
      "email":"info@datacon-bvba.be",
      "country":"Belgium",
      "country_id":"56"
   },
   "delivery":{
      "name":"",
      "address":"",
      "zip":"",
      "city":"",
      "country":""
   },
   "lines":[
      {
         "line":"1",
         "product_id":"8",
         "reference":"MENTOS",
         "reference2":"",
         "product_text":"",
         "description":"MENTOS met een smaakje",   //Update possible
         "costprice":"1.500000",
         "price_vatexcl":5.6132,                   //Update possible
         "price_vatincl":5.95,
         "quantity":3,                             //Update possible
         "delivered":0,                            //Update possible
         "alreadydelivered":0,
         "unit":"ST",
         "unit_multiplier":"1.000000",
         "parent":"0",
         "tax":6,                                  //Update possible
         "discount":0,                             //Update possible
         "subtotal_vat":1.02,
         "subtotal_vatexcl":16.83,
         "subtotal_vatincl":17.85
      },
      {
         "line":"2",
         "product_id":"7675",
         "reference":"LAYSPAP30",
         "reference2":"",
         "product_text":"",
         "description":"Lays paprika 30 g",        //Update possible
         "costprice":"0.500000",
         "price_vatexcl":1.6509,                   //Update possible
         "price_vatincl":1.75,
         "quantity":2,                             //Update possible
         "delivered":1,                            //Update possible
         "alreadydelivered":0,
         "unit":"",
         "unit_multiplier":"1.000000",
         "parent":"0",
         "tax":6,                                  //Update possible
         "discount":0,                             //Update possible
         "subtotal_vat":0.20,
         "subtotal_vatexcl":3.30,
         "subtotal_vatincl":3.5
      }
   ],
   "tax":[
      {
         "tax_pct":"6.0",
         "tax_excl":8.92,
         "tax":0.53,
         "tax_incl":9.45
      }
   ],
   "payments":[
      {
         "type":"8",
         "method":"Webshop",
         "amount":"9.45",
         "date":"2022-11-07"
      }
   ],
   "customfields":null
}
```

This endpoint updates a specific document.

Use the 'lines' array and 'line' value to specify what line of the document to update. If you want to add a line just put 'line:0'.

### HTTPS Request

`PUT https://api.onlinefact.be/documents/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the docmument to update