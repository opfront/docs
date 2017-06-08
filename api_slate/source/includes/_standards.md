# Standards

The Opfront API implements a set of standards intended to make the navigation of resources intuitive and predictable.
These standards describe how resources are accessed, the form of requests and responses, as well as the signification of different status codes.
Unless otherwise specified, you can safely assume that all resources described here will obey those same rules.

## Responses

> Example Successful Response

```json
{
    "status_code": 200,
    "data": {
        "some": "data"
    }
}
```

> Example Error Response

```json
{
    "status_code": 400,
    "message": "Invalid Arguments"
}
```

Every request, regardless of resource or HTTP method, will be formatted the same way.

### Success Response

Field | Type | Description
----- | ---- | -----------
status_code | Int | HTTP Status code of the response
data | Object | Response data

### Error Response

Field | Type | Description
----- | ---- | -----------
status_code | Int | HTTP Status code of the error
message | String | Error message

## Create an Object

> POST /houses

```json
 {
     "address": "1147 Central Street",
     "city": "Brooklyn",
     "state": "Nove Scotia",
     "zip": "B5A 4A8",
     "phone": "902-748-1494"
 }
```

> Response

```json
{
    "status_code": 201,
    "data": {
        "id": 8,
        "address": "1147 Central Street",
        "city": "Brooklyn",
        "state": "Nova Scotia",
        "zip": "B5A 4A8",
        "phone": "902-748-1494"
    }
}
```

### HTTP Request
`POST /{collection}`

### URL Parameters
Parameter | Description
--------- | -----------
collection | Collection in which to create the item

### Request Body
<aside class="warning">Required model fields <b>must</b> be posted for the request to be accepted.</aside>
Any writeable model field can be set in the request body.

## Get an Object

> GET /houses/8

```json
{
    "status_code": 200,
    "data": {
        "id": 8,
        "address": "1147 Central Street",
        "city": "Brooklyn",
        "state": "Nova Scotia",
        "zip": "B5A 4A8",
        "phone": "902-748-1494"
    }
}
```

You can fetch a specific instance of any resource (provided you have access to it).

### HTTP Request
`GET /{collection}/{item_id}`

### URL Parameters
Parameter | Description
--------- | -----------
collection | Name of the collection in which to look for the object
item_id | ID of the object


## List Objects

> GET /houses?offset=6&size=2&summary=true&city=Brooklyn

```json
{
    "status_code": 200,
    "data": {
        "hits": [
            {"id": 7, "address": "2215 Whitmore Road", "city": "Brooklyn"},
            {"id": 8, "address": "1147 Central Street", "city": "Brooklyn"}
        ],
        "offset": 6,
        "size": 2,
        "total": 8
    }
}
```

Every resource offers parameterable paging options and filters via the query string.

### HTTP Request
`GET /{collection}`

### URL Parameters
Parameter | Description
--------- | -----------
collection | Name of the collection to query

### Query Parameters
<aside class="notice">
You can also provide any filterable field as a query parameter to filter the results.
The list of filterable fields will vary depending on the resource. Make sure to check that resource's documentation for a list of filterable fields.
</aside>

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
offset | No | 0 | Number of records by which to offset the resuts
size | No | 10 | Number of records to return
summary | No | False | Whether to return the full resource, or only its summary

## Update an Object

> PATCH /houses/8

```json
{
    "state": "Quebec"
}
```

> Response

```json
{
    "status_code": 200,
    "data": {
        "id": 8,
        "address": "1147 Central Street",
        "city": "Brooklyn",
        "state": "Quebec",
        "zip": "B5A 4A8",
        "phone": "902-748-1494"
    }
}
```
An object can be updated in a variety of ways. Essentially, the object can be updated in its entirety by issuing a `PUT` request, or you can choose
to only update certain fields by issuing a `PATCH` request.

### HTTP Request
`PUT-PATCH /{collection}/{item_id}`

### URL Parameters
Parameter | Description
--------- | -----------
collection | Name of the collection in which to look for the object
item_id | ID of the object

### Request Body
<aside class="notice">The complete update via PUT is subject to the same field requirements as during the creation of the object. </aside>
Any writeable model field can be set in the request body.


## Delete an Object

> DELETE /houses/8

```json
{
    "status_code": 204,
    "data": {}
}
```

### HTTP Request
`DELETE /{collection}/{item_id}`

### URL Parameters
Parameter | Description
--------- | -----------
collection | Name of the collection in which to look for the object
item_id | ID of the object

## Error Codes

The Opfront API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Some fields are missing / invalid
401 | Unauthorized -- No token provided / Invalid token
403 | Forbidden -- Resource for administrators only
404 | Not Found -- Specified resource does not exist
405 | Method Not Allowed -- You tried to access a resource with an invalid HTTP method
409 | Conflict -- You tried to create a resource that already exists
422 | Unprocessable Entity -- The request is well-formed, but some values are invalid
500 | Internal Server Error -- We had a problem with our server. Try again later.
