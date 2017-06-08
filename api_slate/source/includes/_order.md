# Order
An Order represents a client's order of one or more [Products](#product) at a single [Store](#store).

## Client Info Schema
Field | Required | Type | Default | Description
----- | -------- | ---- | ------- | -----------
first_name | ✔️ | String | - | First name
last_name | ✔️ | String | - | Last name
email | ✔️ | String | - | Email address
address | ✔️ | String | - | Home address
phone | ✔️ | String | - | Primary phone number

## Order Status Flow
![test img](images/order_state_machine.png)

## Order Model
Field | Required | Type | Default | Writeable | Filterable | Description
----- | -------- | ---- | ------- | --------- | ---------- | -----------
id | - | Int | - | ✖️ | ✔️ | ID of the order
created_at | - | String | Current Date | ✖️ | ✔️ | Creation date of the order
modified_at | - | String | Current Date | ✖️ | ✔️ | Last modification of the order
product_ids | ✔️ | List\<Int\> | - | ✔️ | ✖️ | IDs of [products](#product) in the order
client_info | ✔️ | [ClientInfo](#client-info-schema) | - | ✔️ | ✖️ | Information on the client making the order
store_id | ✔️ | Int | - | ✔️ | ✔️ | Store where the order is placed
availibilities | ✖️ | Object | {} | ✔️ | ✖️ | Availabilities of the client
status | ✖️ | String | `"new"` | ✔️ | ✔️ | Current [status](#order-status-flow) of the order

## Create an Order

> Example Request

```python
from opfront import OpfrontSession

order_data = {
    'product_ids': [1, 2, 3],
    'client_info': {
        'first_name': 'John',
        'last_name': 'Doe',
        'email': 'john.doe@email.com',
        'address': '1286, Ward Road',
        'phone': '4181234567'
    },
    'store_id': 1,
}

client = OpfrontSession()

new_order = client.order(**order_data)
new_order.save()
```

```shell
curl "/orders"
    -X POST -d @new_order.json \
    -H "Content-Type: application/json" \
```

> Example Response

```json
{
	"data": {
		"availabilities": null,
		"client_info": {
			"address": "1286, Ward Road",
			"email": "john.doe@email.com",
			"first_name": "John",
			"last_name": "Doe",
			"phone": "418234567"
		},
		"created_at": "2017-06-01T18:39:44.241583+00:00",
		"id": 9,
		"modified_at": "2017-06-01T18:39:44.368535+00:00",
		"products": [
			{
                // Product Summary
			}
		],
		"status": "new",
		"store": {
            // Store Summary
		}
	},
	"status_code": 201
}
```

<aside class="notice">This endpoint is public, no need to be authenticated.</aside>

### HTTP Request
`POST https://api.opfront.ca/orders`

### Request Body
<aside class="notice">Make sure to include <b>all</b> of the order's required fields</aside>
All [writeable order fields](#order-model) can be set in the request body.

## Get an Order

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

order_id = 13
order = client.order.get(order_id)
```

```shell
curl "/orders/13"
    -X GET \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"availabilities": null,
		"client_info": {
			"address": "1286, Ward Road",
			"email": "john.doe@email.com",
			"first_name": "John",
			"last_name": "Doe",
			"phone": "418234567"
		},
		"created_at": "2017-06-01T18:39:44.242000+00:00",
		"id": 9,
		"modified_at": "2017-06-01T18:39:44.369000+00:00",
		"products": [
			{
                // Product info
			}
		],
		"status": "new",
		"store": {
            // Store info
		}
	},
	"status_code": 200
}
```

<aside class="notice">You can only fetch a store's order if you are authenticated as an owner of that store.</aside>

### HTTP Request
`GET https://api.opfront.ca/orders/<order_id>`

### URL Parameters
Parameter | Description
--------- | -----------
order_id | The ID of the order to retrieve

## List Orders

> Example Request

```shell
curl "/orders?summary=true&store_id=3"
	-X GET \
    -H "Auth-Token: YOUR_TOKEN"
```

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

# The SDK handles the paging logic automatically
for order in client.order.list(summary=True, store_id=3):
	print(order.product_ids)
```

> Example Response

```json
{
	"data": {
		"hits": [
			{
				"client_info": {
					"address": "69, love station",
					"email": "larry.pee@helloladies.com",
					"first_name": "Larry",
					"last_name": "Party",
					"phone": "123"
				},
				"created_at": "Wed, 31 May 2017 14:19:31 GMT",
				"id": 1,
				"products": [
					{
                        // Product summary
					}
				],
				"status": "new",
				"store": 3
			},
		],
		"offset": 0,
		"size": 10,
		"total": 1
	},
	"status_code": 200
}
```

<aside class="notice">You can only fetch a store's orders if you are authenticated as an owner of that store.</aside>

### HTTP Request
`GET https://api.opfront.ca/orders`

### Query Parameters
All query parameters defined in the [standards](#list-objects) plus:

Parameter | Required | Description
--------- | -------- | -----------
store_id | ✔️ | ID of the store to query

## Update an Order

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

order = client.order.get(18)
order.status = "done"
order.save()
```

```shell
curl "/orders/18"
	-X PATCH -d '{"status":  "done"}' \
	-H "X-Auth-Token: YOUR_TOKEN" \
	-H "Content-Type: application/json"
```

> Example Response

```json
{
	"data": {
		"availabilities": null,
		"client_info": {
			"address": "1286, Ward Road",
			"email": "john.doe@email.com",
			"first_name": "John",
			"last_name": "Doe",
			"phone": "418234567"
		},
		"created_at": "2017-06-01T18:39:44.242000+00:00",
		"id": 18,
		"modified_at": "2017-06-01T18:39:44.369000+00:00",
		"products": [
			{
                // Product info
			}
		],
		"status": "done",
		"store": {
            // Store info
		}
	},
	"status_code": 200
}
```

<aside class="notice">You can only update a store's orders if you are authenticated as an owner of that store.</aside>

### HTTP Request
`PUT-PATCH https://api.opfront.ca/orders/{order_id}`

### URL Parameters
Parameter | Description
--------- | -----------
order_id | ID of the order to update

### Request Body
<aside class="notice">The complete update via PUT is subject to the same field requirements as during the creation of the order. </aside>
All [writeable order fields](#order-model) can be set in the request body.

## Delete an Order

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

an_order = client.order.get(18)
an_order.delete()
# OR
client.order.delete(18)
```

```shell
curl "/orders/18"
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

<aside class="notice">You can only delete a store's orders if you are authenticated as an owner of that store.</aside>

### HTTP Request
`DELETE https://api.opfront.ca/orders/<order_id>`

### URL Parameters
Parameter | Description
--------- | -----------
order_id | The ID of the order to delete
