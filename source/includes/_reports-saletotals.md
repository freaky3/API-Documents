## Sale totals per day

```php

$ch = curl_init('https://api.onlinefact.be/reports/saletotals/?min_date=2022-07-27&max_date=2022-07-28');
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
curl "https://api.onlinefact.be/reports/saletotals/?min_date=2022-07-27&max_date=2022-07-28" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "date":"2022-07-27",
      "document_type":8,
      "checkoutnumber":0,
      "filter":null,
      "total_tax_excl":2258.77,
      "total_tax":135.54,
      "total_tax_incl":2394.31,
      "transactions":101,
      "tax_totals":[
         {
            "percentage":6,
            "total_tax_excl":2258.77,
            "total_tax":135.54,
            "total_tax_incl":2394.31
         }
      ],
      "payments":[
         {
            "payment_method":2,
            "amount":884.62
         },
         {
            "payment_method":4,
            "amount":76.75
         },
         {
            "payment_method":6,
            "amount":1267.45
         },
         {
            "payment_method":13,
            "amount":82.68
         },
         {
            "payment_method":20,
            "amount":82.8
         }
      ]
   },
   {
      "date":"2022-07-28",
      "document_type":8,
      "checkoutnumber":0,
      "filter":null,
      "total_tax_excl":2209.21,
      "total_tax":132.52,
      "total_tax_incl":2341.73,
      "transactions":85,
      "tax_totals":[
         {
            "percentage":6,
            "total_tax_excl":2209.21,
            "total_tax":132.52,
            "total_tax_incl":2341.73
         }
      ],
      "payments":[
         {
            "payment_method":2,
            "amount":817.55
         },
         {
            "payment_method":4,
            "amount":49.3
         },
         {
            "payment_method":6,
            "amount":1269.9
         },
         {
            "payment_method":13,
            "amount":167.6
         },
         {
            "payment_method":20,
            "amount":37.5
         }
      ]
   }
]
```

This endpoint retrieves the sale totals per day.

### HTTPS Request

`GET https://api.onlinefact.be/reports/saletotals/`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
min_date | date | Return sales for a specific start date, the date need to be in the YYYY-MM-DD format `[mandatory]`
max_date | date | Return sales for a specific end date, the date need to be in the YYYY-MM-DD format `[mandatory]`
document_type | integer |1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket
checkoutnumber | integer | POS number
filter | string | Filter result with string data *(customer reference/name)*

### Result Parameters

Parameter | Type | Description
--------- | ------- | -----------
document_type | integer | Type of document (1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket)
checkoutnumber | integer | POS number *(0 = all)*
total_tax_excl | decimal | Total of day excl vat
total_tax | decimal | Total of day vat
total_tax_incl | decimal | Total of day incl vat
transactions | integer | Total number of transactions (documents)
tax_totals | array | Totals per vat percentage
payments | array | Totals per payment method (1 = not payd, 2 = cash, 3 = banktransfer, 4 = creditcard, 5 = cheque, 6 = bancontact, 8 = webshop, 11 = paypal, 99 = voucher)
