# Bank Transactions

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
open_amount | decimal | Unsigned open amount: the absolute value of `amount` minus the linked payments. The result is never negative.
bank_account_id | integer | Id of the own bank account this transaction belongs to.
account_iban | string | IBAN of the own bank account.
bank_account | object | Own bank account. See **Bank transactions - bank_account**.
payments | array | Payments linked to this transaction. See **Bank transactions - payments**.

**Bank transactions - bank_account**

Parameter | Type | Description
--------- | ------- | -----------
id | integer | Bank account id.
name | string | Account name. Currently always `null`.
iban | string | IBAN of the bank account.
bic | string | BIC of the bank account.

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
amount | decimal | Amount of the payment applied to this document.

<aside class="notice">
Only <code>GET</code> is supported. Authenticate with HTTP Basic authentication: the API key is the username and the API secret is the password. Results are limited to bank accounts of the account of the API key.
</aside>

## Get All Bank Transactions

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";

$ch = curl_init('https://api.onlinefact.be/banktransactions/?limit=2&page=1&min_date=2026-03-01&max_date=2026-03-04&direction=out&onlyopen=1&bank_account_id=12&order_by=date&order_dir=desc');
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
curl "https://api.onlinefact.be/banktransactions/?limit=2&page=1&min_date=2026-03-01&max_date=2026-03-04&direction=out&onlyopen=1&bank_account_id=12&order_by=date&order_dir=desc" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "id":9021,
      "date":"2026-03-04",
      "amount":"-120.50",
      "direction":"out",
      "counterparty_name":"Leverancier NV",
      "counterparty_iban":"BE71096123456769",
      "message":"Factuur 2026/014",
      "reference":"+++000/1234/56789+++",
      "open_amount":"20.50",
      "bank_account_id":12,
      "account_iban":"BE68539007547034",
      "bank_account":{
         "id":12,
         "name":null,
         "iban":"BE68539007547034",
         "bic":"KREDBEBB"
      },
      "payments":[
         {
            "payment_id":245,
            "amount":"100.00",
            "date":"2026-03-04",
            "documents":[
               {
                  "document_id":510,
                  "document_type":11,
                  "document_number":"2026/014",
                  "amount":"100.00"
               }
            ]
         }
      ]
   },
   {
      "id":9014,
      "date":"2026-03-02",
      "amount":"-45.00",
      "direction":"out",
      "counterparty_name":"Energie BV",
      "counterparty_iban":"BE62510007547061",
      "message":"Voorschot maart",
      "reference":"",
      "open_amount":"45.00",
      "bank_account_id":12,
      "account_iban":"BE68539007547034",
      "bank_account":{
         "id":12,
         "name":null,
         "iban":"BE68539007547034",
         "bic":"KREDBEBB"
      },
      "payments":[

      ]
   }
]
```

This endpoint retrieves all bank transactions.

`open_amount` is unsigned. For the first transaction, `abs(-120.50) - 100.00 = 20.50`.

### HTTPS Request

`GET https://api.onlinefact.be/banktransactions/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
min_date | | minimum transaction date (`YYYY-MM-DD`). Alias: `from`.
max_date | | maximum transaction date (`YYYY-MM-DD`). Alias: `to`.
direction | | `in` or `out`. Alias: `outgoing=1` selects outgoing transactions (`direction=out`).
onlyopen | | `1` returns only transactions that still have an open amount. Alias: `open=1`.
bank_account_id | | only transactions for this bank account id.
limit | 100 | limit results (max 1000).
page | 1 | page number of result.
order_by | id | sort by `id`, `date` or `amount`.
order_dir | ASC | sort direction `ASC` or `DESC`.

## Get a Specific Bank Transaction

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";
$bank_transaction_id = 9021;

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
curl "https://api.onlinefact.be/banktransactions/9021/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
   "id":9021,
   "date":"2026-03-04",
   "amount":"-120.50",
   "direction":"out",
   "counterparty_name":"Leverancier NV",
   "counterparty_iban":"BE71096123456769",
   "message":"Factuur 2026/014",
   "reference":"+++000/1234/56789+++",
   "open_amount":"20.50",
   "bank_account_id":12,
   "account_iban":"BE68539007547034",
   "bank_account":{
      "id":12,
      "name":null,
      "iban":"BE68539007547034",
      "bic":"KREDBEBB"
   },
   "payments":[
      {
         "payment_id":245,
         "amount":"100.00",
         "date":"2026-03-04",
         "documents":[
            {
               "document_id":510,
               "document_type":11,
               "document_number":"2026/014",
               "amount":"100.00"
            }
         ]
      }
   ]
}
```

This endpoint retrieves a specific bank transaction.

### HTTPS Request

`GET https://api.onlinefact.be/banktransactions/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the bank transaction to retrieve.
