# Bank Transactions

`GET /banktransactions/` returns a JSON array of bank accounts. Each account contains the transactions that match the filters. Accounts with no matching transactions stay in the list with `transactions` set to `[]`.

Parameter | Type | Description
--------- | ------- | -----------
id | integer | Unique identifier for the bank account. `[read-only]`
name | string | Account name as set in the backoffice. `null` when empty.
iban | string | IBAN of the bank account.
bic | string | BIC of the bank account.
currency | string | Currency of the bank account, for example `EUR`.
institution_id | string | Identifier of the bank institution.
active | boolean | `true` when the bank account is active. Inactive accounts (`active` = 0) are omitted unless `include_inactive=1`.
balance | decimal | Current balance of the bank account.
last_sync | datetime | Timestamp of the last synchronization (`YYYY-MM-DD HH:MM:SS`).
connection_valid | boolean | `true` when the bank connection is valid.
total_transactions | integer | Number of matching transactions for this account across all pages.
transactions | array | Transactions on this page. See **Bank transactions - transaction**.

**Bank transactions - transaction**

Parameter | Type | Description
--------- | ------- | -----------
id | integer | Unique identifier for the bank transaction. `[read-only]`
date | date | Transaction date (`YYYY-MM-DD`).
amount | decimal | Signed transaction amount. Outgoing amounts are negative.
direction | string | `in` for money received, `out` for money paid.
counterparty_name | string | Name of the counterparty.
counterparty_iban | string | IBAN of the counterparty.
message | string | Message or remittance information.
reference | string | Structured or free reference.
open_amount | decimal | Unsigned open amount: the absolute value of `amount` minus the absolute amounts of the linked payments.
payments | array | Payments linked to this transaction. See **Bank transactions - payments**.

**Bank transactions - payments**

Parameter | Type | Description
--------- | ------- | -----------
payment_id | integer | Id of the linked payment.
amount | decimal | Amount of the payment linked to this transaction.
date | date | Payment date (`YYYY-MM-DD`).
documents | array | Documents this payment is applied to. See **Bank transactions - payments - documents**.

**Bank transactions - payments - documents**

Parameter | Type | Description
--------- | ------- | -----------
document_id | integer | Document id.
document_type | integer | Document type id. `11` is a purchase invoice.
document_number | string | Document number.
amount | decimal | Amount recorded for this document on the payment.

<aside class="notice">
Only <code>GET</code> is supported. Authenticate with HTTP Basic authentication: the API key is the username and the API secret is the password. Results are limited to the account of the API key. Filters, <code>limit</code>, <code>page</code>, <code>order_by</code> and <code>order_dir</code> apply to transactions. There is a next page when <code>page * limit</code> is lower than the sum of <code>total_transactions</code>.
</aside>

## Get All Bank Transactions

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";

$ch = curl_init('https://api.onlinefact.be/banktransactions/?limit=100&page=1&min_date=2026-10-01&max_date=2026-10-17&direction=out&onlyopen=1&order_by=date&order_dir=desc');
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, $api_key.":".$api_secret);
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/banktransactions/?limit=100&page=1&min_date=2026-10-01&max_date=2026-10-17&direction=out&onlyopen=1&order_by=date&order_dir=desc" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "id":7,
      "name":"KBC",
      "iban":"BE11000000000001",
      "bic":"KREDBEBB",
      "currency":"EUR",
      "institution_id":"KBC_KREDBEBB",
      "active":true,
      "balance":"1520.40",
      "last_sync":"2026-10-17 08:12:03",
      "connection_valid":true,
      "total_transactions":1,
      "transactions":[
         {
            "id":11,
            "date":"2026-10-05",
            "amount":"-80.50",
            "direction":"out",
            "counterparty_name":"Supplier NV",
            "counterparty_iban":"BE71096123456769",
            "message":"Factuur 2026/15",
            "reference":"narrative raw",
            "open_amount":"50.25",
            "payments":[
               {
                  "payment_id":82,
                  "amount":"-30.25",
                  "date":"2026-10-05",
                  "documents":[
                     {
                        "document_id":501,
                        "document_type":11,
                        "document_number":"AF2026/15",
                        "amount":"80.50"
                     }
                  ]
               }
            ]
         }
      ]
   }
]
```

This endpoint retrieves bank accounts for the API key, with the transactions that match the filters nested in `transactions`.

Accounts without matching transactions stay in the list with `transactions` set to `[]`. `open_amount` for the transaction above is `abs(-80.50) - abs(-30.25) = 50.25`.

### HTTPS Request

`GET https://api.onlinefact.be/banktransactions/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
min_date | | minimum transaction date (`YYYY-MM-DD`). Alias: `from`.
max_date | | maximum transaction date (`YYYY-MM-DD`). Alias: `to`.
direction | | `in` or `out`. Alias: `outgoing=1` selects outgoing transactions (`direction=out`).
onlyopen | | `1` returns only transactions that still have an open amount.
bank_account_id | | return only this bank account. Alias: `iban` (the numeric bank account id, not the IBAN string).
include_inactive | | inactive accounts (`active` = 0) are omitted unless this is `1`.
limit | 100 | limit transaction results (max 1000).
page | 1 | page number of transaction results.
order_by | id | sort transactions by `id`, `date` or `amount`.
order_dir | ASC | sort direction `ASC` or `DESC`.

## Get a Specific Bank Transaction

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";
$bank_transaction_id = 11;

$ch = curl_init('https://api.onlinefact.be/banktransactions/'.$bank_transaction_id.'/');
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_USERPWD, $api_key.":".$api_secret);
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
$response = curl_exec($ch);
curl_close($ch);

$result = json_decode($response, true);

print_r($result);
```

```shell
curl "https://api.onlinefact.be/banktransactions/11/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
   "id":7,
   "name":"KBC",
   "iban":"BE11000000000001",
   "bic":"KREDBEBB",
   "currency":"EUR",
   "institution_id":"KBC_KREDBEBB",
   "active":true,
   "balance":"1520.40",
   "last_sync":"2026-10-17 08:12:03",
   "connection_valid":true,
   "total_transactions":1,
   "transactions":[
      {
         "id":11,
         "date":"2026-10-05",
         "amount":"-80.50",
         "direction":"out",
         "counterparty_name":"Supplier NV",
         "counterparty_iban":"BE71096123456769",
         "message":"Factuur 2026/15",
         "reference":"narrative raw",
         "open_amount":"50.25",
         "payments":[
            {
               "payment_id":82,
               "amount":"-30.25",
               "date":"2026-10-05",
               "documents":[
                  {
                     "document_id":501,
                     "document_type":11,
                     "document_number":"AF2026/15",
                     "amount":"80.50"
                  }
               ]
            }
         ]
      }
   ]
}
```

This endpoint retrieves one bank account object. `transactions` contains only the requested transaction.

### HTTPS Request

`GET https://api.onlinefact.be/banktransactions/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the bank transaction to retrieve.
