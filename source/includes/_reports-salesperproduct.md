## Sales per product

```php

$ch = curl_init('https://api.onlinefact.be/reports/salesperproduct/?min_date=2022-07-27&max_date=2022-07-28');
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
curl "https://api.onlinefact.be/reports/salesperproduct/?min_date=2022-07-27&max_date=2022-07-28" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "product_id":1477,
      "reference":"1490",
      "reference2":"",
      "description":"ziesner curry ketchup 800 ml ",
      "category":"default",
      "subcategory":"",
      "stock":15,
      "purchase_price":0,
      "cost_price":0,
      "price_vat_excl":4.7075,
      "price_vat_incl":4.99,
      "tax":6,
      "sum_qty":1,
      "average_price_excl":4.71,
      "average_price_incl":4.99,
      "total_price_excl":4.71,
      "total_price_incl":4.99,
      "search_query":{
         "document_type":8,
         "checkoutnumber":0,
         "date_min":"2022-11-02",
         "date_max":"2022-11-03",
         "filter":null
      }
   },
   {
      "product_id":4892,
      "reference":"5471",
      "reference2":"",
      "description":"ZEISNER TOMATEN KETCHUP 800 ML ",
      "category":"default",
      "subcategory":"",
      "stock":8,
      "purchase_price":0,
      "cost_price":0,
      "price_vat_excl":4.7075,
      "price_vat_incl":4.99,
      "tax":6,
      "sum_qty":2,
      "average_price_excl":4.71,
      "average_price_incl":4.99,
      "total_price_excl":9.42,
      "total_price_incl":9.98,
      "search_query":{
         "document_type":8,
         "checkoutnumber":0,
         "date_min":"2022-11-02",
         "date_max":"2022-11-03",
         "filter":null
      }
   }
]
```

This endpoint retrieves the total products sold in a period.

### HTTPS Request

`GET https://api.onlinefact.be/reports/salesperproduct/`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
min_date | date | Return sales for a specific start date, the date need to be in the YYYY-MM-DD format `[mandatory]`
max_date | date | Return sales until a specific end date, the date need to be in the YYYY-MM-DD format `[mandatory]`
document_type | integer |1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket
checkoutnumber | integer | POS number
opendocuments | integer | 0 = all documents, 1 = only from open documents
filter | string | Filter result with string data *(product reference/name, supplier, category name, brand name)*

### Result Parameters

Parameter | Type | Description
--------- | ------- | -----------

