# Categories

Parameter | Type | Description
--------- | ------- | -----------
category_id | integer | Unique identifier for the resource. `[read-only]`
parent_id | integer | id of parent category
name | string | Name of category.
product_count | integer | number of products in category. `[read-only]`
category_stock | decimal | items in stock in this category. `[read-only]`
datemodified | date-time | date and time of last modification `[read-only]`
sort_order | integer| sequence of display this categorie `[read-only]` *(can only be changed in de backoffice)*

## Get All Categories

```php

$ch = curl_init('https://api.onlinefact.be/categories/');
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
curl "https://api.onlinefact.be/categories/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
[
   {
      "category_id":"5",
      "parent_id":"0",
      "name":"Clothes",
      "product_count":"1",
      "category_stock":"26.00",
      "sort_order":"1",
      "datemodified":"2022-10-25 11:47:14"
   },
   {
      "category_id":"6",
      "parent_id":"5",
      "name":"T-shirts",
      "product_count":"1",
      "category_stock":"10.00",
      "sort_order":"2",
      "datemodified":"2022-10-25 11:47:14"
   }
]
```

This endpoint retrieves all categories.

### HTTPS Request

`GET https://api.onlinefact.be/categories/`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | limit results (max 1000).
page | 1 | page number of result.


## Get a Specific Category

```php

$ch = curl_init('https://api.onlinefact.be/categories/5/');
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
curl "https://api.onlinefact.be/categories/5/" \
  -X GET \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```

> The above command returns JSON structured like this:

```json
{
    "category_id":"5",
    "parent_id":"0",
    "name":"Clothes",
    "product_count":"1",
    "category_stock":"26.00",
    "sort_order":"1",
    "datemodified":"2022-10-25 11:47:14"
}
```

This endpoint retrieves a specific category.

### HTTPS Request

`GET https://api.onlinefact.be/categories/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the category to retrieve


## Add a Category

```php

$data_string = '{
                  "name":"Schoes",
                  "parent_id":5
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/categories/");
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
curl "https://api.onlinefact.be/categories/" \
  -X POST \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "name":"Shoes",
        "parent_id":5
      }'
```

> The above command returns JSON structured like this:

```json
{
    "category_id":"8",
    "parent_id":"5",
    "name":"Shoes",
    "product_count":"0",
    "category_stock":"0.00",
    "sort_order":"6",
    "datemodified":"2022-10-25 11:47:14"
}
```

This endpoint add a categorie.

### HTTPS Request

`POST https://api.onlinefact.be/categories/`

### URL Parameters

Parameter | Description
--------- | -----------
name | name of the categorie
parent_id | id of parent category

## Update a categorie

```php

$data_string = '{
                  "name":"Jeans"
                }'; //JSON String

$ch = curl_init("https://api.onlinefact.be/categories/8/");
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
curl "https://api.onlinefact.be/categories/8/" \
  -X PUT \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
  -d '{
        "name":"Jeans",
      }'
```

> The above command returns JSON structured like this:

```json
{
    "category_id":"8",
    "parent_id":"5",
    "name":"Jeans",
    "product_count":"0",
    "category_stock":"0.00",
    "sort_order":"6",
    "datemodified":"2022-10-25 11:47:14"
}
```

This endpoint updates a specific category.

### HTTPS Request

`PUT https://api.onlinefact.be/categories/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the catgory to update


## Delete a Category

```php
$ch = curl_init("https://api.onlinefact.be/categories/8/");
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
curl "https://api.onlinefact.be/categories/8/" \
  -X DELETE \
  -u "api_key_here:api_secret_here" \
  -H "Content-Type: application/json" \
```


> The above command returns JSON structured like this:

```json
{
  "category_id": "8",
  "deleted" : "success"
}
```

This endpoint deletes a specific category.

### HTTPS Request

`DELETE https://api.onlinefact.be/categories/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the category to delete
