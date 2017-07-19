# Banner

The Banner resource is the highest-level entity in our system. A banner represents a collection of stores under the same brand. Each banner is managed by a single owner
(altough we have plans to allow multiple owners in the future), who may or may not be a store owner.

## Banner Model

Field | Required | Type | Default | Writeable | Filterable | Description
----- | -------- | ---- | ------- | --------- | ---------- | -----------
id | - | Int | - | ✖️ | ✔️ | ID of the banner
created_at | - | String | Current Date | ✖️ | ✔️ | Creation date of the banner
modified_at | - | String | Current Date | ✖️ | ✔️ | Last modification of the banner
name | ✔️ | String | - | ✔️ | ✔️ | Name of the banner
owner_id | ✔️ | Int | - | ✖️ | ✔️ | ID of the banner owner
billing_city | ✖️ | String | `null` | ✔️ | ✔️ | Billing city
billing_province | ✖️ | String | `null` | ✔️ | ✔️ | Billing province
billing_country | ✖️ | String | `null` | ✔️ | ✔️ | Billing country
billing\_postal\_code | ✖️ | String | `null` | ✔️ | ✔️ | Billing postal code
billing_address | ✖️ | String | `null` | ✔️ | ✔️ | Billing street address
billing_email | ✖️ | String | `null` | ✔️ | ✔️ | Email to use for billing

## Get a Banner

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

banner_id = 13
banner = client.banner.get(banner_id)
```

```shell
curl "/banners/13"
    -X GET
    -H "X-Auth-Token: AUTH_TOKEN"
```

> Example Response

```json
{
	"data": {
		"billing_address": null,
		"billing_city": "Quebec",
		"billing_country": null,
		"billing_email": null,
		"billing_postal_code": null,
		"billing_province": null,
		"created_at": "2017-07-06T17:15:39.165000+00:00",
		"id": 2,
		"modified_at": "2017-07-06T17:15:39.165000+00:00",
		"name": "My Banner",
		"owner_id": 1
	},
	"status_code": 200
}
```

<aside class="notice">You can only fetch a banner if you are authenticated as an owner of that banner.</aside>

### HTTP Request
`GET https://api.opfront.ca/banners/{banner_id}`

### URL Parameters
Parameter | Description
--------- | -----------
banner | The ID of the banner to retrieve

## List Banners

> Example Request

```shell
curl "/banners?summary=true&owner_id=1"
	-X GET \
    -H 'X-Auth-Token: YOUR_TOKEN'
```

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

# The SDK handles the paging logic automatically
for banner in client.banner.list(summary=True, owner_id=1):
	print(banner.name)
```

> Example Response

```json
{
	"data": {
		"hits": [
			{
				"billing_address": null,
				"billing_city": "Quebec",
				"billing_country": null,
				"billing_email": null,
				"billing_postal_code": null,
				"billing_province": null,
				"created_at": "2017-07-06T17:15:39.165000+00:00",
				"id": 2,
				"modified_at": "2017-07-06T17:15:39.165000+00:00",
				"name": "My Banner",
				"owner_id": 1
			}
		],
		"offset": 0,
		"size": 10,
		"total": 1
	},
	"status_code": 200
}
```

### HTTP Request
`GET https://api.opfront.ca/banners`

### Query Parameters
All query parameters defined in the [standards](#list-objects) plus:

Parameter | Required | Description
--------- | -------- | -----------
owner_id  | ✔️ | ID of the owner by which to filter the banners

<aside class="notice">You need to be logged in as the user by which you filter the banners.</aside>

## Update a Banner

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
    email="test@test.ca",
    password="12345"
)

banner_id = 1
banner = client.banner.get(banner_id)

banner.name = "New Banner Name"
banner.save()
```

```shell
curl "/banners/1"
    -X PATCH -d '{"name": "New Banner Name"}' \
    -H 'X-Auth-Token: YOUR_TOKEN' \
    -H 'Content-Type: application/json' \
```

### HTTP Request
`PUT-PATCH https://api.opfront.ca/banners/{banner_id}`

### URL Parameters
Parameter | Description
--------- | -----------
banner_id | ID of the banner to update

### Request Body
<aside class="notice">The complete update via PUT is subject to the same field requirements as during the creation of the banner. </aside>
All [writeable banner fields](#banner-model) can be set in the request body.

