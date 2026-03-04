# Vouchers

Parameter | Type | Description
--------- | ------- | -----------
voucher_id | integer | Unique identifier for the resource. `[read-only]` 
barcode | string | Barcode of the voucher.
name | string | name of the voucher.
datecreated | date | date when voucher is created. `[read-only]` 
amount | decimal| current value of the voucher.
transactions | array |

**vouchers - transactions**

Parameter | Type | Description
--------- | ------- | -----------
transaction_id | integer | Unique identifier for the resource. `[read-only]`
datecreated | date | date when transaction is created. `[read-only]`
amount | decimal| amount of change + or - 
document_id | integer | ID of the document where the transaction is on. `[read-only]`
document_number | string | document of this transaction. `[read-only]`
comment | string | extra comment of transaction.
 
## Get All Vouchers

```php

$ch = curl_init('https://api.onlinefact.be/vouchers/');
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
curl "https://api.onlinefact.be/vouchers" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
    {
      "voucher_id":"60",
      "barcode":"2000000015606",
      "name":"Tegoedbon",
      "datecreated":"2022-11-03",
      "amount":0,
      "transactions":[
         {
            "transaction_id":"77",
            "datecreated":"2022-11-03",
            "amount":"17.90",
            "document_id":123,
            "document_number":"Ticket 2022\/236",
            "comment":""
         },
         {
            "transaction_id":"78",
            "datecreated":"2022-11-03",
            "amount":"-17.90",
            "document_id":124,
            "document_number":"Ticket 2022\/237",
            "comment":""
         }
      ]
   },
   {
      "voucher_id":"59",
      "barcode":"2000000015590",
      "name":"Cadeaubon",
      "datecreated":"2022-11-03",
      "amount":50,
      "transactions":[
         {
            "transaction_id":"76",
            "datecreated":"2022-11-03",
            "amount":"50.00",
            "document_id":126,
            "document_number":"Ticket 2022\/232",
            "comment":""
         }
      ]
   }
]
```

This endpoint retrieves all vouchers.

### HTTPS Request

`GET https://api.onlinefact.be/vouchers/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000).
page | 1 | page number of result.
barcode | | search by barcode
from_datemodified || vouchers changed sinds this timestamp

## Get a Specific Voucher

```php

$ch = curl_init('https://api.onlinefact.be/vouchers/59/');
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
curl "https://api.onlinefact.be/vouchers/59/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
    "voucher_id":"59",
    "barcode":"2000000015590",
    "name":"Cadeaubon",
    "datecreated":"2022-11-03",
    "amount":50,
    "transactions":[
        {
        "transaction_id":"76",
        "datecreated":"2022-11-03",
        "amount":"50.00",
        "document_id":126,
        "document_number":"Ticket 2022\/232",
        "comment":""
        }
    ]
}
```

This endpoint retrieves a specific voucher.

### HTTPS Request

`GET https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the vouchers to retrieve

<aside class="notice">
Get voucher by barcode: https://api.onlinefact.be/vouchers/?barcode=2000000015606`
</aside>

## Add a Voucher

```php

$data_string = '{
                  "name":"Gift Voucher",
                  "amount":25.00,
                  "comment":"webshop order #123"
               }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/vouchers/");
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
curl "https://api.onlinefact.be/vouchers/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
         "name":"Gift Voucher",
         "amount":25.00,
         "comment":"webshop order #123"
      }'
```

> The above command returns JSON structured like this:

```json
{
   "voucher_id":"99",
   "barcode":"2000000015606",
   "name":"Gift Voucher",
   "datecreated":"2022-11-03",
   "amount":25.00,
   "transactions":[
      {
         "transaction_id":"125",
         "datecreated":"2022-11-03",
         "amount":25.00,
         "document_id":0,
         "document_number":"Manual",
         "comment":"webshop order #123"
      },
   ]
}
```

This endpoint add a voucher.

### HTTPS Request

`POST https://api.onlinefact.be/vouchers/`

### URL Parameters

Parameter | Description
--------- | -----------
name | name of the voucher.
amount | value of the voucher
barcode | barcode of the voucher (not mandatory)
comment | extra comment of the transcation

## Update a Voucher

```php

$data_string = '{
                  "amount":-10.00,
                  "comment":"Webshop order #150"
               }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/vouchers/99/");
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
curl "https://api.onlinefact.be/vouchers/23/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
         "amount":-10.00,
         "comment":"Webshop order #150"
      }'
```

> The above command returns JSON structured like this:

```json
{
   "voucher_id":"99",
   "barcode":"2000000015606",
   "name":"Gift Voucher",
   "datecreated":"2022-11-03",
   "amount":15.00,
   "transactions":[
      {
         "transaction_id":"125",
         "datecreated":"2022-11-03",
         "amount":25.00,
         "document_id":0,
         "document_number":"Manual",
         "comment":"webshop order #123"
      },
      {
         "transaction_id":"155",
         "datecreated":"2022-11-09",
         "amount":-10.00,
         "document_id":0,
         "document_number":"Manual",
         "comment":"Webshop order #150"
      },
   ]
}
```

This endpoint updates a specific voucher.

### HTTPS Request

`PUT https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the voucher to update
amount | amount of change + or -
comment | extra comment of the transcation

## Delete a Voucher

```php
$ch = curl_init("https://api.onlinefact.be/vouchers/99/");
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
curl "https://api.onlinefact.be/vouchers/99/" \
  -X DELETE \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```


> The above command returns JSON structured like this:

```json
{
  "voucher_id": "99",
  "deleted" : "success"
}
```

This endpoint deletes a specific voucher.

### HTTPS Request

`DELETE https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the voucher to delete
