# Store

The store resource essentially represents a physical selling location, with it's own inventory. For now, stores have only one owner,
but we have plans to support multiple owners soon.

## Store Model
Field | Required | Type | Default | Writeable | Filterable | Description
----- | -------- | ---- | ------- | --------- | ---------- | -----------
id | - | Int | - | ✖️ | ✔️ | ID of the store
created_at | - | String | Current Date | ✖️ | ✔️ | Creation date of the store
modified_at | - | String | Current Date | ✖️ | ✔️ | Last modification of the store
name | ✔️ | String | - | ✔️ | ✔️ | Store name
phone | ✔️ | String | - | ✔️ | ✔️ | Store phone number
street_address | ✔️ | String | - | ✔️ | ✔️ | Street address of the store
city | ✔️ | String | - | ✔️ | ✔️ | City
province | ✔️ | String | - | ✔️ | ✔️ | Province
postal_code | ✔️ | String | - | ✔️ | ✔️ | Store zip code
owner_id | ✔️ | Int | - | ✔️ | ✔️ | ID of the store's owner
banner_id | ✔️ | Int | - | ✔️ | ✔️ | ID of the store's banner
currency | ✖️ | String | `"CAD"` | ✔️ | ✔️ | Currency in use by the store
email | ✖️ | String | `null` | ✔️ | ✔️ | Store email (`null` when same as owner email)
domain_name | ✖️ | String | `null` | ✔️ | ✔️ | Domain name of the store (`null` when domain unknown)
country | ✖️ | String | `Canada` | ✔️ | ✔️ | Store country

## Create a Store

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
	email="test@test.ca",
	password="12345"
)

STORE_DATA = {
	'name': 'Test Store',
	'phone': '1234567890',
	'street_address': '12 Test',
	'city': 'Quebec',
	'province': 'QC',
	'postal_code': 'A1A 1A1',
	'owner_id': 1,
	'banner_id': 1
}

new_store = client.store(**STORE_DATA)
```

```shell
curl "/stores"
    -X POST -d @new_store.json \
    -H "Content-Type: application/json" \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"banner": {
			"billing_city": "Quebec",
			"billing_country": "Canada",
			"billing_province": "QC",
			"id": 1,
			"name": "TestBanner"
		},
		"city": "Quebec",
		"country": "Canada",
		"created_at": "2017-07-05T15:21:37.286224+00:00",
		"currency": "CAD",
		"domain_name": null,
		"email": null,
		"id": 15,
		"modified_at": "2017-07-05T15:21:37.286256+00:00",
		"name": "Test Store",
		"owner_id": 1,
		"phone": "1234567890",
		"postal_code": "A1A 1A1",
		"province": "QC",
		"street_address": "12 Test"
	},
	"status_code": 201
}
```

<aside class="notice">You can only create a store in a banner if you are authenticated as an owner of that banner.</aside>

### HTTP Request
`POST https://api.opfront.ca/stores`

### Request Body
<aside class="notice">Make sure to include <b>all</b> of the store's required fields</aside>
All [writeable store fields](#store-model) can be set in the request body.

## Get a Store

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

store_id = 13
store = client.store.get(store_id)
```

```shell
curl "/stores/13"
    -X GET \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"city": "Quebec",
		"country": "canada",
		"created_at": "2017-05-31T14:19:31.658000+00:00",
		"currency": "CAD",
		"domain_name": "leo-opto.com",
		"email": "leo@leo-optp.ca",
		"id": 13,
		"last_sync": "2017-05-31T14:19:31.658000+00:00",
		"modified_at": "2017-05-31T14:19:31.658000+00:00",
		"name": "Leo opto",
		"owner_id": 1,
		"banner_id": 1,
		"phone": "4446568989",
		"postal_code": "g1g1g1",
		"province": "qc",
		"street_address": "226 joseph est"
	},
	"status_code": 200
}
```

<aside class="notice">You can only fetch a store if you are authenticated as an owner of that store.</aside>

### HTTP Request
`GET https://api.opfront.ca/stores/<store_id>`

### URL Parameters
Parameter | Description
--------- | -----------
store_id | The ID of the store to retrieve

## List Stores

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

for store in client.store.list(summary=True, banner_id=1):
	print(store.street_address)
```

```shell
curl "/stores?summary=true&banner_id=3"
	-X GET \
    -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"hits": [
			{
				"city": "Quebec",
				"country": "Canada",
				"email": null,
				"id": 16,
				"name": "Demo Store",
				"phone": "1234567890",
				"postal_code": "G1G 1G1",
				"province": "QC",
				"street_address": "18 place ave"
			}
		],
		"offset": 0,
		"size": 10,
		"total": 4
	},
	"status_code": 200
}
```

### HTTP Request
`GET https://api.opfront.ca/stores`

### Query Parameters
All query parameters defined in the [standards](#list-objects) plus:

<aside class="notice">You must be logged in as an owner of the banner by which you filter for the request to work.</aside>

Parameter | Required | Description
--------- | -------- | -----------
banner_id | ✔️ | ID of the banner to query

## Update a Store

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

store = client.store.get(18)
store.street_address = '12 test rd'
store.save()
```

```shell
curl "/stores/18"
	-X PATCH -d '{"street_address": "12 test rd"}' \
	-H "X-Auth-Token: YOUR_TOKEN" \
	-H "Content-Type: application/json"
```

> Example Response

```json
{
	"data": {
		"banner": {
			"billing_city": "Quebec",
			"billing_country": null,
			"billing_province": null,
			"id": 1,
			"name": "OptoBanner"
		},
		"city": "Quebec",
		"country": "canada",
		"created_at": "2017-07-04T18:41:15.287000+00:00",
		"currency": "CAD",
		"domain_name": "leo-opto.com",
		"email": "leo@leo-optp.ca",
		"id": 1,
		"modified_at": "2017-07-05T15:51:48.601194+00:00",
		"name": "Leo opto",
		"owner_id": 2,
		"phone": "4446568989",
		"postal_code": "g1g1g1",
		"province": "qc",
		"street_address": "12 test rd"
	},
	"status_code": 200
}
```

### HTTP Request
`PUT-PATCH https://api.opfront.ca/stores/{store_id}`

### URL Parameters
Parameter | Description
--------- | -----------
store_id | ID of the store to update

### Request Body
<aside class="notice">The complete update via PUT is subject to the same field requirements as during the creation of the store. </aside>
All [writeable store fields](#store-model) can be set in the request body.


## Delete a Store

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

store_id = 12
store = client.store.get(store_id)

store.delete()
# OR
client.store.delete(store_id)
```

```shell
curl "/stores/12"
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
`DELETE https://api.opfront.ca/stores/{store_id}`

### URL Parameters
Parameter | Description
--------- | -----------
store_id | ID of the store to delete
