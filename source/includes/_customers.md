# Customers

Parameter | Type | Description
--------- | ------- | -----------
customer_id | integer | Unique identifier for the resource. `[read-only]`
reference | string | Unique code.
taxnr | string | National TAX number.
name | string | Name or company.
name2 | string | 
address | string | Address of customer
address2 | string | 
zip | string | Postal code for invoice
city | string | City for invoice
country_id | integer | <a href="https://en.wikipedia.org/wiki/ISO_3166-1_numeric" target="_blank">ISO 3166-1</a> numeric code of the country 
country_iso | integer | <a href="https://en.wikipedia.org/wiki/ISO_3166-2" target="_blank">ISO 3166-2</a> alpha-2 code of the country 
language_id | integer | 0 = default, 1 = NL, 2 = FR, 3 = EN, 4 = DE, 6 = TR
mobile | string | Mobile phone number
phone | string | Landline phone number
email | string | Default email address of customer
birthdate | date | Birthdate of customer
type | integer | 1 = customer, 2 = supplier, 3 = customer and supplier
expirationdays | integer | Default days when document expires
discount | decimal | fixed discount percentage on all products
barcode | string | barcode of the loyalty card
loyalty_points | integer | number of loyalty points collected
delivery_name | string | Delivery address name
delivery_address | string | Delivery address
delivery_zip | string | Delivery address zip
delivery_city | string | Delivery address city
delivery_country | string | Delivery address country name
datemodified | date-time | date and time of last modification `[read-only]`
 

## Get All Customers

```php

$ch = curl_init('https://api.onlinefact.be/customers/');
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
curl "https://api.onlinefact.be/customers" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
  {
    "customer_id":"23",
    "reference":"DATACON",
    "taxnr":"BE0894069190",
    "name":"BVBA DATACON",
    "name2":"",
    "address":"Schootstraat 191",
    "address2":"",
    "zip":"3550",
    "city":"Heusden-Zolder",
    "country_id":"56",
    "country_iso":"BE",
    "mobile":"",
    "phone":"+3211743319",
    "email":"info@datacon-bvba.be",
    "birthdate":"1981-12-31",
    "type":"3",
    "expirationdays":"14",
    "discount":"0.00",
    "barcode":"1000123",
	"loyalty_points":10,
    "delivery_name":"",
    "delivery_address":"",
    "delivery_zip":"",
    "delivery_country":"",
    "datemodified":"2022-10-25 11:47:14"
  },
  {
    "customer_id":"67",
    "reference":"ONLINEFACT",
    "taxnr":"BE0123456789",
    "name":"Onlinefact",
    "name2":"Kristof Moons",
    "address":"Schootstraat 191",
    "address2":null,
    "zip":"3550",
    "city":"Heusden-Zolder",
    "country_id":"56",
    "country_iso":"BE",
    "mobile":"011743319",
    "phone":"",
    "email":"info@onlinefact.be",
    "birthdate":"0000-00-00",
    "type":"1",
    "expirationdays":"14",
    "discount":"0.00",
    "barcode":"1000124",
	"loyalty_points":35,
    "delivery_name":null,
    "delivery_address":null,
    "delivery_zip":null,
    "delivery_country":null,
    "datemodified":"2022-10-22 11:41:10"
  }
]
```

This endpoint retrieves all customers.

### HTTPS Request

`GET https://api.onlinefact.be/customers/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000).
page | 1 | page number of result.
from_id || customer added sinds this customer_id
from_datemodified || customers changed sinds this timestamp
name || part of name
address || part of address
email || 
phone || part of phone number
mobile || part of mobile number

## Get a Specific Customer

```php

$ch = curl_init('https://api.onlinefact.be/customers/23/');
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
curl "https://api.onlinefact.be/customers/23/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
  "customer_id":"23",
  "reference":"DATACON",
  "taxnr":"BE0894069190",
  "name":"BVBA DATACON",
  "name2":"",
  "address":"Schootstraat 191",
  "address2":"",
  "zip":"3550",
  "city":"Heusden-Zolder",
  "country_id":"56",
  "country_iso":"BE",
  "mobile":"",
  "phone":"+3211743319",
  "email":"info@datacon-bvba.be",
  "birthdate":"1981-12-31",
  "type":"3",
  "expirationdays":"14",
  "discount":"0.00",
  "barcode":"1000123",
  "loyalty_points":10,
  "delivery_name":"",
  "delivery_address":"",
  "delivery_zip":"",
  "delivery_country":"",
  "datemodified":"2022-10-25 11:47:14"
}
```

This endpoint retrieves a specific customer.

### HTTPS Request

`GET https://api.onlinefact.be/customers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the customer to retrieve


## Add a Customer

```php

$data_string = '{
                  "reference":"DATACON",
                  "taxnr":"BE0894069190",
                  "name":"BVBA DATACON",
                  "address":"Kerkstraat 123",
                  "zip":"1000",
                  "city":"Brussels",
                  "country_id":"56",
                  "phone":"+3211743319",
                  "email":"info@datacon-bvba.be",
                  "birthdate":"1981-12-31",
                  "type":"3"
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/customers/");
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
curl "https://api.onlinefact.be/customers/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "reference":"DATACON",
        "taxnr":"BE0894069190",
        "name":"BVBA DATACON",
        "address":"Kerkstraat 123",
        "zip":"1000",
        "city":"Brussels",
        "country_id":"56",
        "phone":"+3211743319",
        "email":"info@datacon-bvba.be",
        "birthdate":"1981-12-31",
        "type":"3"
      }'
```

> The above command returns JSON structured like this:

```json
{
  "customer_id":"23",
  "reference":"DATACON",
  "taxnr":"BE0894069190",
  "name":"BVBA DATACON",
  "name2":"",
  "address":"Kerkstraat 123",
  "address2":"",
  "zip":"1000",
  "city":"Brussels",
  "country_id":"56",
  "country_iso":"BE",
  "mobile":"",
  "phone":"+3211743319",
  "email":"info@datacon-bvba.be",
  "birthdate":"1981-12-31",
  "type":"3",
  "expirationdays":"14",
  "discount":"0.00",
  "barcode":"1000123",
  "delivery_name":"",
  "delivery_address":"",
  "delivery_zip":"",
  "delivery_country":"",
  "datemodified":"2022-10-25 11:47:14"
}
```

This endpoint add a customer.

### HTTPS Request

`POST https://api.onlinefact.be/customers/`

### URL Parameters

Parameter | Description
--------- | -----------
reference | Unique code (if empty Onlinefact will generate unique id)


## Update a Customer

```php

$data_string = '{
                  "address":"Kerkstraat 123",
                  "zip":"1000",
                  "city":"Brussels",
				  "loyalty_points":{"action":"add","amount":10}
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/customers/23/");
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
curl "https://api.onlinefact.be/customers/23/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "address":"Kerkstraat 123",
        "zip":"1000",
        "city":"Brussels",
		"loyalty_points":{"action":"add","amount":10}
      }'
```

> The above command returns JSON structured like this:

```json
{
  "customer_id":"23",
  "reference":"DATACON",
  "taxnr":"BE0894069190",
  "name":"BVBA DATACON",
  "name2":"",
  "address":"Kerkstraat 123",
  "address2":"",
  "zip":"1000",
  "city":"Brussels",
  "country_id":"56",
  "country_iso":"BE",
  "mobile":"",
  "phone":"+3211743319",
  "email":"info@datacon-bvba.be",
  "birthdate":"1981-12-31",
  "type":"3",
  "expirationdays":"14",
  "discount":"0.00",
  "barcode":"1000123",
  "loyalty_points":"10",
  "delivery_name":"",
  "delivery_address":"",
  "delivery_zip":"",
  "delivery_country":"",
  "datemodified":"2022-10-28 10:59:59"
}
```

This endpoint updates a specific customer.

### HTTPS Request

`PUT https://api.onlinefact.be/customers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the customer to update

**Customer - loyalty_points**

Parameter | Type | Description
--------- | ------- | -----------
action | string | what todo with the amount (add,substract,new_amount)
amount | integer | amount of modification of loyalty_points


## Delete a Customer

```php
$ch = curl_init("https://api.onlinefact.be/customers/23/");
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
curl "https://api.onlinefact.be/customers/23/" \
  -X DELETE \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```


> The above command returns JSON structured like this:

```json
{
  "customer_id": "23",
  "deleted" : "success"
}
```

This endpoint deletes a specific customer.

### HTTPS Request

`DELETE https://api.onlinefact.be/customers/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the customer to delete
