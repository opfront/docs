# Store

The store resource essentially represents a physical selling location, with it's own inventory. Every store in the system
has to pick a [template](#templates) that will dictate the look and feel of the website generated for it. For now, stores have only one owner,
but we have plans to support multiple owners soon.

## Templates
Currently available website templates:

* `reflex-template`
* `fovea`

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
template | ✔️ | String | - | ✔️ | ✔️ | Name of the [template](#templates) in use by the store
currency | ✖️ | String | `"CAD"` | ✔️ | ✔️ | Currency in use by the store
domain_name | ✖️ | String | `null` | ✔️ | ✔️ | Domain name of the store (`null` when domain unknown)
ehr_type | ✖️ | String | `null` | ✔️ | ✔️ | EHR in use in the store (`null` when EHR unknown)
ehr_api_url | ✖️ | String | `null` | ✔️ | ✔️ | URL for the store's EHR (`null` when EHR unknown / no API)
ehr_api_key | ✖️ | String | `null` | ✔️ | ✔️ | Key for the store's EHR API (`null` when no EHR API)
last_sync | ✖️ | String | `null` | ✔️ | ✔️ | Timestamp of last EHR sync (`null` when no sync was performed yet)
billing_street_address | ✖️ | String | `null` | ✔️ | ✔️ | Store billing address (`null` when same as store address)
billing_city | ✖️ | String | `null` | ✔️ | ✔️ | Store billing city (`null` when same as store city)
billing_province | ✖️ | String | `null` | ✔️ | ✔️ | Store billing province (`null` when same as store province)
billing_country | ✖️ | String | `null` | ✔️ | ✔️ | Store billing country (`null` when same as store country)
billing_postal_code | ✖️ | String | `null` | ✔️ | ✔️ | Store billing zip code (`null` when same as store zip code)
billing_email | ✖️ | String | `null` | ✔️ | ✔️ | Store billing email (`null` when same as owner email)

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
		"billing_address": null,
		"billing_city": null,
		"billing_country": null,
		"billing_email": null,
		"billing_postal_code": null,
		"billing_province": null,
		"billing_street_address": null,
		"city": "Quebec",
		"country": "canada",
		"created_at": "2017-05-31T14:19:31.658000+00:00",
		"currency": "CAD",
		"domain_name": "leo-opto.com",
		"ehr_api_key": null,
		"ehr_api_url": "leo-opto.optpsys.com/this/is/an/api/url",
		"ehr_type": "optosys",
		"email": "leo@leo-optp.ca",
		"id": 13,
		"last_sync": "2017-05-31T14:19:31.658000+00:00",
		"modified_at": "2017-05-31T14:19:31.658000+00:00",
		"name": "Leo opto",
		"owner_id": 1,
		"phone": "4446568989",
		"postal_code": "g1g1g1",
		"province": "qc",
		"street_address": "226 joseph est",
		"template": "reflex-template"
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
