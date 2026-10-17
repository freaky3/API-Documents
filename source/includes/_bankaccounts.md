# Bank Accounts

Parameter | Type | Description
--------- | ------- | -----------
id | integer | Unique identifier for the bank account. `[read-only]`
name | string | Account name. Currently always `null`.
iban | string | IBAN of the bank account.
bic | string | BIC of the bank account.
balance | decimal | Current balance of the bank account.
last_sync | datetime | Timestamp of the last successful synchronization (`YYYY-MM-DD HH:MM:SS`).
connection_valid | boolean | `true` when the bank connection is valid.
expires_at | datetime | Expiry of the bank connection. Currently always `null`.
last_http_code | integer | HTTP status code of the last synchronization request to the bank.

<aside class="notice">
Only <code>GET</code> is supported. Authenticate with HTTP Basic authentication: the API key is the username and the API secret is the password. Results are limited to the account of the API key.
</aside>

## Get All Bank Accounts

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";

$ch = curl_init('https://api.onlinefact.be/bankaccounts/?limit=2&page=1&order_by=iban&order_dir=asc');
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
curl "https://api.onlinefact.be/bankaccounts/?limit=2&page=1&order_by=iban&order_dir=asc" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "id":12,
      "name":null,
      "iban":"BE68539007547034",
      "bic":"KREDBEBB",
      "balance":"2450.75",
      "last_sync":"2026-03-04 08:15:22",
      "connection_valid":true,
      "expires_at":null,
      "last_http_code":200
   },
   {
      "id":18,
      "name":null,
      "iban":"BE71096123456769",
      "bic":"GEBABEBB",
      "balance":"180.10",
      "last_sync":"2026-03-03 22:01:05",
      "connection_valid":false,
      "expires_at":null,
      "last_http_code":401
   }
]
```

This endpoint retrieves all bank accounts.

### HTTPS Request

`GET https://api.onlinefact.be/bankaccounts/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000).
page | 1 | page number of result.
order_by | id | sort by `id`, `iban`, `balance` or `last_sync`.
order_dir | ASC | sort direction `ASC` or `DESC`.

## Get a Specific Bank Account

```php

$api_key = "api_key_here";
$api_secret = "api_secret_here";
$bank_account_id = 12;

$ch = curl_init('https://api.onlinefact.be/bankaccounts/'.$bank_account_id.'/');
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
curl "https://api.onlinefact.be/bankaccounts/12/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
   "id":12,
   "name":null,
   "iban":"BE68539007547034",
   "bic":"KREDBEBB",
   "balance":"2450.75",
   "last_sync":"2026-03-04 08:15:22",
   "connection_valid":true,
   "expires_at":null,
   "last_http_code":200
}
```

This endpoint retrieves a specific bank account.

### HTTPS Request

`GET https://api.onlinefact.be/bankaccounts/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the bank account to retrieve.
