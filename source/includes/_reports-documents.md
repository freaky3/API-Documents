## Documents list

```php

$ch = curl_init('https://api.onlinefact.be/reports/documents/?min_date=2022-07-27&max_date=2022-07-28');
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
curl "https://api.onlinefact.be/reports/documents/?min_date=2022-07-27&max_date=2022-07-28" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "document_type":8,
      "document_id":707,
      "document_number":"2023\/105",
      "closed":0,
      "document_date":"2022-07-27",
      "document_time":"14:33:05",
      "document_expiration":"2023-01-30",
      "datemodified": "2022-11-07 11:18:44",
      "OGM":"001\/1583\/50354",
      "document_reference":"",
      "document_comments":"",
      "customer_id":2,
      "customer_reference":"REG",
      "taxnr":"",
      "name":"Cash Register 14:33",
      "name2":"",
      "address":"",
      "address2":"",
      "zip":"",
      "city":"",
      "country_id":21,
      "country":"Belgium",
      "mobile":"",
      "phone":"",
      "email":"",
      "user_created":"2",
      "user_created_name":"",
      "checkout_number":3,
      "document_cashdisc":0,
      "total_tax_excl":250,
      "total_tax_incl":250,
      "profit":250,
      "saldo":0,
      "payment_status":"Cash",
      "payments":[
         {
            "Type":2,
            "Method":"Cash",
            "Amount":250,
            "Date":"2022-07-27"
         }
      ],
      "tax_totals":[
         {
            "percentage":0,
            "total_tax_excl":250,
            "total_tax":0,
            "total_tax_incl":250
         }
      ],
      "search_query":{
         "document_type":8,
         "checkoutnumber":"3",
         "date_min":"2022-07-27",
         "date_max":"2022-07-28",
         "filter":null
      }
   },
   {
      "document_type":8,
      "document_id":708,
      "document_number":"2023\/106",
      "closed":0,
      "document_date":"2022-07-28",
      "document_time":"14:51:18",
      "document_expiration":"2023-01-30",
      "datemodified": "2022-11-07 11:18:44",
      "OGM":"001\/1599\/88745",
      "document_reference":null,
      "customer_id":201,
      "customer_reference":"K201",
      "document_comments":"",
      "taxnr":"BE0123456789",
      "name":"Datacon BVBA",
      "name2":"",
      "address":"Schootstraat 191",
      "address2":"",
      "zip":"3550",
      "city":"Heusden-Zolder",
      "country_id":21,
      "country":"Belgium",
      "mobile":"",
      "phone":"",
      "email":"",
      "user_created":"2",
      "user_created_name":"Kristof",
      "checkout_number":3,
      "document_cashdisc":0,
      "total_tax_excl":41.32,
      "total_tax_incl":50,
      "profit":41.32,
      "saldo":50,
      "payment_status":"Te Betalen",
      "payments":[
         
      ],
      "tax_totals":[
         {
            "percentage":21,
            "total_tax_excl":41.32,
            "total_tax":8.68,
            "total_tax_incl":50
         }
      ],
      "search_query":{
         "document_type":8,
         "checkoutnumber":"3",
         "date_min":"2022-07-27",
         "date_max":"2022-07-28",
         "filter":null
      }
   }
]
```

This endpoint retrieves the document list for a specific period.

### HTTPS Request

`GET https://api.onlinefact.be/reports/documents/`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
min_date | date | Return sales for a specific start date, the date need to be in the YYYY-MM-DD format `[mandatory]`
max_date | date | Return sales for a specific end date, the date need to be in the YYYY-MM-DD format `[mandatory]`
document_type | integer |1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket
onlyopen | integer | 1 = only open documents, 0 = open and closed documents
customer_reference | string | only documents with this customer reference
filter | string | Filter result with string data *(customer reference/name)*

### Result Parameters

Parameter | Type | Description
--------- | ------- | -----------
document_type | integer | Type of document (1 = offer, 2 = client order, 3 = invoice, 4 = creditnote, 5 = deliverynote, 8 = ticket)
document_id | integer | Internal document ID of the document 
document_number | varchar | Number of the document
closed | integer | is document closed
document_date | date | 
document_time | time | 
document_expiration | date | 
datemodified | datetime | timestamp of last modification
OGM | varchar | 
document_reference | varchar | 
document_comments | varchar | 
customer_id | integer | 
customer_reference | varchar | 
taxnr | varchar | 
name | varchar | 
name2 | varchar | 
address | varchar | 
address2 | varchar | 
zip | varchar | 
city | varchar | 
country_id | integer | 
country | varchar | 
mobile | varchar | 
phone | varchar | 
email | varchar | 
user_created | integer | 
user_created_name | varchar | 
checkout_number | integer | 
document_cashdisc | decimal | 
total_tax_excl | decimal | Total of day excl vat
total_tax_incl | decimal | Total of day incl vat
profit | decimal | 
saldo | decimal | 
payment_status | varchar | 
payments | array | Totals per payment method (1 = not payd, 2 = cash, 3 = banktransfer, 4 = creditcard, 5 = cheque, 6 = bancontact, 8 = webshop, 11 = paypal, 99 = voucher)
tax_totals | array | Totals per vat percentage

