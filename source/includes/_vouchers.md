# Vouchers

<aside class="warning">
Draft, not rolled out. Storing <code>customer_id</code> requires the nullable column from <code>database/migrations/2026-11-06_vouchers_custid.sql</code>. The negative <code>PUT</code> balance check runs only when <code>vouchers</code> and <code>voucher_transactions</code> are both InnoDB. Without that engine the old insert still allows a balance below zero, the migration keeps the column, exits non-zero, and does not convert the engine. Existing anonymous vouchers stay <code>customer_id: null</code> and keep working by barcode. This draft does not stop double redemption on a POS ticket: the till still stores the full negative amount, so the balance can go below zero, including when two tickets or two tills use the same voucher. Refusing that redemption before any write is a release blocker and is not implemented. The <code>PUT</code> check is not that guard.
</aside>

Authenticate with HTTP Basic authentication. The API key is the username and the API secret is the password. Every customer id and voucher id belongs to that account only.

Parameter | Type | Description
--------- | ------- | -----------
voucher_id | integer | Unique identifier for the resource. `[read-only]` 
barcode | string | Barcode of the voucher.
name | string | name of the voucher.
datecreated | date | date when voucher is created. `[read-only]` 
customer_id | integer | Optional owner (`vouchers.custid`). `null` when the voucher is anonymous. An integer is a relation in this account. `[read-write]`
amount | decimal | Current balance of the voucher: the sum of its transaction amounts.
transactions | array |

On create, omit `customer_id` or send `null`, `""` or `0` and the voucher stays anonymous. Old clients that leave the field out are unchanged. On update, omit `customer_id` to leave the current owner unchanged; `null` or `0` clears the owner. An empty string does not assign an owner. Setting or clearing an owner does not change any document.

**vouchers - transactions**

Parameter | Type | Description
--------- | ------- | -----------
transaction_id | integer | Unique identifier for the resource. `[read-only]`
datecreated | date | date when transaction is created. `[read-only]`
amount | decimal| amount of change + or - 
document_id | integer | ID of the document where the transaction is on. `[read-only]`
document_number | string | document of this transaction. `[read-only]`
comment | string | extra comment of transaction.

Voucher errors below are HTTP 200 with JSON, in the same style as the existing voucher API:

```json
{"success": 0, "error": "customer_id not found"}
```

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

```shell
curl "https://api.onlinefact.be/vouchers/?customer_id=55&open=1&limit=100&page=1" \
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
      "customer_id": null,
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
      "customer_id": null,
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

> `GET /vouchers/?customer_id=55&open=1&limit=100&page=1` returns only open vouchers of that relation. One row looks like this:

```json
[
  {
    "voucher_id": 90,
    "barcode": "2111111111118",
    "name": "Tegoed",
    "datecreated": "2026-11-06 10:00:00",
    "customer_id": 55,
    "amount": 15,
    "transactions": []
  }
]
```

This endpoint retrieves all vouchers. A list is a JSON array. One barcode match stays a single object, as today.

### HTTPS Request

`GET https://api.onlinefact.be/vouchers/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000). Applied after `open=1`.
page | 1 | page number of result. Applied after `open=1`.
barcode | | search by barcode
from_datemodified || vouchers changed sinds this timestamp
customer_id | | Only vouchers owned by this relation in this account. An unknown id returns `{"success":0,"error":"customer_id not found"}`, not an empty list. An id from another account is the same error.
open | | `open=1` keeps only vouchers with a balance greater than zero. `limit` and `page` apply after that filter.

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
    "customer_id": null,
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

This endpoint retrieves a specific voucher. The object includes `customer_id` (`null` when the voucher is anonymous).

### HTTPS Request

`GET https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the vouchers to retrieve

<aside class="notice">
Get voucher by barcode: https://api.onlinefact.be/vouchers/?barcode=2000000015606
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

```shell
curl "https://api.onlinefact.be/vouchers/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
         "barcode": "",
         "name": "Tegoed",
         "amount": 25.00,
         "comment": "kassa",
         "customer_id": 55
      }'
```

> The above command returns JSON structured like this:

```json
{
   "voucher_id":"99",
   "barcode":"2000000015606",
   "name":"Gift Voucher",
   "datecreated":"2022-11-03",
   "customer_id": null,
   "amount":25.00,
   "transactions":[
      {
         "transaction_id":"125",
         "datecreated":"2022-11-03",
         "amount":25.00,
         "document_id":0,
         "document_number":"Manual",
         "comment":"webshop order #123"
      }
   ]
}
```

> A successful create with an owner returns one object, the same shape as GET of that voucher:

```json
{
  "voucher_id": 90,
  "barcode": "2111111111118",
  "name": "Tegoed",
  "datecreated": "2026-11-06 10:00:00",
  "customer_id": 55,
  "amount": 25,
  "transactions": [
    {
      "transaction_id": 1,
      "datecreated": "2026-11-06 10:00:00",
      "amount": "25.00",
      "document_id": 0,
      "document_number": "Manual",
      "comment": "kassa"
    }
  ]
}
```

This endpoint add a voucher. `amount` greater than zero books the opening balance, as today. A negative `amount` on POST is not booked. That is unchanged. An empty `barcode` makes the server create an EAN, as today. If that barcode already exists, the response is `Add voucher failed` and the owner of the existing voucher does not change.

`customer_id` is optional. Omit it, or send `null`, `""` or `0`, and the voucher stays anonymous.

An unknown id, an id from another account, or relation `REG` (the cash-register account) cannot be an owner. The voucher is not created:

```json
{"success": 0, "error": "customer_id not found"}
```

Before `vouchers.custid` exists, the same call does not create the voucher:

```json
{"success": 0, "error": "voucher_owner_not_migrated"}
```

### HTTPS Request

`POST https://api.onlinefact.be/vouchers/`

### URL Parameters

Parameter | Description
--------- | -----------
name | name of the voucher.
amount | value of the voucher. Greater than zero books the opening balance. A negative amount is not booked.
barcode | barcode of the voucher (not mandatory). Empty: the server creates an EAN.
comment | extra comment of the transcation
customer_id | Optional owner in this account. Omit, `null`, `""` or `0` to keep the voucher anonymous.

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

```shell
curl "https://api.onlinefact.be/vouchers/90/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
         "customer_id": null,
         "amount": 5,
         "comment": "bijboeking"
      }'
```

> The above command returns JSON structured like this:

```json
{
   "voucher_id":"99",
   "barcode":"2000000015606",
   "name":"Gift Voucher",
   "datecreated":"2022-11-03",
   "customer_id": null,
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
      }
   ]
}
```

This endpoint updates a specific voucher. A successful PUT returns the voucher object (the same GET of that id), including `customer_id`. This call does not create a sales line. A refusal therefore cannot leave a voucher-paid ticket behind.

Without `customer_id`, a positive or zero `amount` stays the existing insert. The owner does not change.

A negative `amount` is refused only when `vouchers` and `voucher_transactions` are both InnoDB and the balance would go below zero. The check locks the voucher row (`SELECT … FROM vouchers … FOR UPDATE`). When balance plus amount is below zero, the update is rolled back: no transaction row is written, and no smaller amount is stored. `balance` in the error is the current balance, for example `4.00` when that is what remains:

```json
{"success": 0, "error": "insufficient voucher balance", "balance": "4.00"}
```

If that request also sends `customer_id`, the owner stays unchanged. Nothing from the refused movement is written.

If either table is not InnoDB, that refusal does not run and the negative amount is inserted as before. The balance may go below zero. `FOR UPDATE` is not a lock unless both tables are InnoDB. Two equal manual amounts with the same comment are both stored when the balance allows each of them. The lock does not cover a POS ticket. POS redemption still writes the full negative amount and can take the balance below zero.

`customer_id` on PUT sets or clears the owner. `null` or `0` clears the owner. An empty string does not assign an owner. Omitting `customer_id` leaves the current owner in place. An unknown id, an id from another account, or relation `REG` returns `customer_id not found`. Before the column exists, PUT with `customer_id` returns `voucher_owner_not_migrated` and does not store the owner.

### HTTPS Request

`PUT https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the voucher to update
amount | amount of change + or -. A negative amount is refused only when both voucher tables are InnoDB and the balance would go below zero. Otherwise the full amount is stored, and without InnoDB the balance may go below zero.
comment | extra comment of the transcation
customer_id | Optional. A relation id in this account sets the owner. `null` or `0` clears the owner. Omit the field to leave the owner unchanged.

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

This endpoint deletes a specific voucher and its transactions for this account. It does not change documents. Behaviour is otherwise unchanged.

### HTTPS Request

`DELETE https://api.onlinefact.be/vouchers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the voucher to delete

## Owner migration

Storing `customer_id` depends on `database/migrations/2026-11-06_vouchers_custid.sql`. An operator runs it once per tenant database that contains `vouchers` (the database from `account2db` / `dbs`). Do not run it on the control database unless that database itself has `vouchers`. The script copies the type of `custumers.custid`, adds the column as `NULL`, fills nothing, and does not change documents.

After that, both `vouchers` and `voucher_transactions` must be InnoDB. If they are not, the script exits with a non-zero status and `InnoDB required on vouchers and voucher_transactions; custid column is kept`. The column stays. Running the script again repeats the check. The script does not convert the storage engine.

Until the column exists, anonymous sale and barcode redemption keep working. `customer_id` on `POST /vouchers/` or `PUT /vouchers/<ID>` returns `voucher_owner_not_migrated`. POST does not create the voucher. PUT does not store the owner. The negative-amount refusal on PUT also stays off until both tables are InnoDB. POS ticket redemption does not use that refusal.
