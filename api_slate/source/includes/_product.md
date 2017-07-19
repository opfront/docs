# Product
The Product resource is basically a cross-product between a [Store](#store) and a [Spectacle](#spectacle).
It represents an inventory entry of a single spectacle for a single store.

## Product Model
Field | Required | Type | Default | Writeable | Filterable | Description
----- | -------- | ---- | ------- | --------- | ---------- | -----------
id | - | Int | - | ✖️ | ✔️ | ID of the product
created_at | - | String | Current Date | ✖️ | ✔️ | Creation date of the product
modified_at | - | String | Current Date | ✖️ | ✔️ | Last modification of the product
name | ✔️ | String | - | ✔️ | ✔️ | Name of the product
external_id | ✔️ | String | - | ✔️ | ✔️ | ID of the product in the store's system
description | ✔️ | String | - | ✔️ | ✔️ | Description of the product
store_id | ✔️ | Int | - | ✔️ | ✔️ | ID of the store to which the product belongs
spectacle_id |✖️| String | - | ✔️ | ✔️ | ID of the spectacle attached to the product
quantity | ✖️ | Int | `null` | ✔️ | ✔️ | In-store product quantity (`null` when quantity unknown)
price | ✖️ | Float | `null` | ✔️ | ✔️ | Price of the product (`null` when price unknown)
external_sku | ✖️ | String | `null` | ✔️ | ✔️ | SKU of the product in the store's inventory
synced | ✖️ | Boolean | true | ✔️ | ✔️ | Whether the product is synced
is_publicly_visible | ✖️ | Boolean | false | ✔️ | ✔️ | Whether the product is publicly listed

## Create a Product

> Example Request

```python
from opfront import OpfrontSession

product_data = {
    'name': 'Test product',
    'external_id': '42',
    'description': 'A product',
    'store_id': 1,
    'spectacle_id': "583eede09712a6643d2a0d19"
}

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

new_product = client.product(**product_data)
new_product.save()
```

```shell
curl "/products"
    -X POST -d @new_product.json \
    -H "Content-Type: application/json" \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"created_at": "2017-05-31T19:52:16.896669+00:00",
		"description": "A Product.",
		"external_id": "42",
		"external_sku": null,
		"id": 6,
		"is_publicly_visible": false,
		"modified_at": "2017-05-31T19:52:16.896695+00:00",
		"name": "Test Product",
		"price": null,
		"quantity": null,
		"spectacle": {
            // Spectacle Data
		},
		"store_id": 1,
		"synced": true
	},
	"status_code": 201
}
```

### HTTP Request
`POST https://api.opfront.ca/products`

### Request Body
<aside class="notice">Make sure to include <b>all</b> of the product's required fields</aside>
All [writeable product fields](#product-model) can be set in the request body.


## Get a Product

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession()

product_id = 13
product = client.product.get(product_id)
```

```shell
curl "/products/13"
    -X GET
```

> Example Response

```json
{
	"data": {
		"created_at": "2017-05-31T19:52:16.897000+00:00",
		"description": "A Product.",
		"external_id": "42",
		"external_sku": null,
		"id": 6,
		"is_publicly_visible": false,
		"modified_at": "2017-05-31T19:52:16.897000+00:00",
		"name": "Test Product",
		"price": null,
		"quantity": null,
		"spectacle": {
            // Spectacle Data
        },
		"store_id": 1,
		"synced": true
	},
	"status_code": 200
}
```

<aside class="notice">This endpoint is public, no need to be authenticated.</aside>

### HTTP Request
`GET https://api.opfront.ca/products/<product_id>`

### URL Parameters
Parameter | Description
--------- | -----------
product_id | The ID of the product to retrieve

## List Products

> Example Request

```shell
curl "/products?summary=true&store_id=3&synced=False"
	-X GET
```

```python
from opfront import OpfrontSession

client = OpfrontSession()

# The SDK handles the paging logic automatically
for product in client.product.list(summary=True, store_id=3, synced=False):
	print(product.name)
```

> Example Response

```json
{
	"data": {
		"hits": [
			{
				"id": 6,
				"name": "Test Product",
				"quantity": null,
				"spectacle": {
					// Spectacle Summary
				}
			}
		],
		"offset": 0,
		"size": 10,
		"total": 1
	},
	"status_code": 200
}
```

<aside class="notice">This endpoint is public, no need to be authenticated.</aside>

### HTTP Request
`GET https://api.opfront.ca/products`

### Query Parameters
All query parameters defined in the [standards](#list-objects) plus:

<aside class="notice">It is mandatory to specify store_id <i>or</i> banner_id, no need to specify both.</aside>

Parameter | Required | Description
--------- | -------- | -----------
store_id | ✔️ | ID of the store to query
banner_id | ✔️ | ID of the banner to query
spec_filters | ✖️ | URL-encoded JSON filter object used to filter products by their spectacle's attributes


## Update a Product

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

product = client.product.get(18)
product.name = 'New Name'
product.save()
```

```shell
curl "/products/18"
	-X PATCH -d '{"name": "New Name"}' \
	-H "X-Auth-Token: YOUR_TOKEN" \
	-H "Content-Type: application/json"
```

### HTTP Request
`PUT-PATCH https://api.opfront.ca/products/{product_id}`

### URL Parameters
Parameter | Description
--------- | -----------
product_id | ID of the product to update

### Request Body
<aside class="notice">The complete update via PUT is subject to the same field requirements as during the creation of the product. </aside>
All [writeable product fields](#product-model) can be set in the request body.


## Delete a Product

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

a_product = client.product.get(18)
a_product.delete()
# OR
client.product.delete(18)
```

```shell
curl "/products/18"
    -X DELETE \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
    "status_code": 204,
    "data": {}
}
```

### HTTP Request
`DELETE https://api.opfront.ca/products/<product_id>`

### URL Parameters
Parameter | Description
--------- | -----------
product_id | The ID of the product to delete
