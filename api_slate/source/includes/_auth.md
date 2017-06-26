# Authentication

Our API is authenticated via [JSON Web Tokens](https://jwt.io/). Before making requests on the different endpoints offered by the API,
you must first acquire an authentication token (used for making authenticated requests) as well as a refresh token (used to get a new token once it expires).

Unless specified, Opfront expects for the authentication token to be included with all API requests to the server in a header that looks like the following:

`X-Auth-Token: zxcvzxcv`

<aside class="notice">
You must replace <code>zxcvzxcv</code> with your own authentication token.
</aside>

## First Login

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
  email="user@domain.com",
  password="12345asdf"
)
```

```shell
curl "/auth/"
  -X POST -d '{"email": "user@domain.com", "password": "12345asdf"}'\
  -H "Content-Type: application/json"
```

> Example Response

```json
{
  "data": {
    "auth_token": "TOKEN_HERE",
    "refresh_token": "TOKEN_HERE"
  },
  "status_code": 200
}
```

<aside class="warning">
Authentication tokens expire after a period of six hours, after which you will need to fetch a new one
by using the refresh token
</aside>

### HTTP Request
`POST https://api.opfront.ca/auth/login`

### Request Body
Parameter | Required | Type | Default | Description
--------- | -------- | ---- | ------- | -----------
email | Yes | String | - | Email address of the user you wish to authenticate as.
password | Yes | String | - | Password of the user you wish to authenticate as.

## Refresh Authentication Token

> Example Request

```python
# Handled automatically by SDK
```

```javascript
// Handled automatically by SDK
```

```shell
curl "/auth/refresh"
  -X POST -d '{"refresh_token": "TOKEN_HERE"}' \
  -H "Content-Type: application/json
```

> Example Response

```json
{
  "auth_token": "NEW_TOKEN"
}
```

### HTTP Request
`POST https://api.opfront.ca/auth/refresh`

### Request Body
Parameter | Required | Type | Default | Description
--------- | -------- | ---- | ------- | -----------
refresh_token | Yes | String | - | Refresh token for which to generate an auth token.

## Fetch Current User
Our API also exposes a way to fetch the profile of an authenticated user.

> Example Request

```python
from opfront import OpfrontSession

client = OpfrontSession(
  email="user@domain.com",
  password="12345asdf"
)

user = client.user.me()
```

```javascript
```

```shell
curl "/users/me"
  -X GET \
  -H "X-Auth-Token: YOUR_TOKEN"
```

> Example Response

```json
{
	"data": {
		"city": "Quebec",
		"confirmed": true,
		"country": "Canada",
		"created_at": "2017-06-22T14:11:46.818000+00:00",
		"date_of_birth": null,
		"email": "test2@opfront.ca",
		"eyesight_condition": null,
		"face_type": null,
		"first_name": "John",
		"gender": "M",
		"id": 12,
		"is_admin": false,
		"last_name": "Doe",
		"modified_at": "2017-06-22T14:11:46.818000+00:00",
		"phone": null,
		"postal_code": null,
		"province": "QC",
		"street_address": null
	},
	"status_code": 200
}
```

### HTTP Request

`GET https://api.opfront.ca/users/me`
