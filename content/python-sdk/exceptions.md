+++
date = "2017-06-02T16:39:35-04:00"
title = "Exceptions"
toc = true
weight = 10

+++

The python SDK defines a few custom exceptions to make the handling of API error codes more pythonic:

### `ResourceNotFoundError`
*Bases: Exception*

`ResourceNotFoundError` is the SDK equivalent of an HTTP 404. It means that the resource you tried
to fetch does not exist.

### `BadRequestError`
*Bases: Exception*

`BadRequestError` is the SDK equivalent of an HTTP 400. It means that you tried to create or update a resource
with an invalid payload. Refer to the [api documentation](/api) for more details on the different models in use by the Opfront API.

### `UnexpectedError`
*Bases: Exception*

`UnexpectedError` reflects an unexpected failure on our end, it is the SDK equivalent of an HTTP 500. Simply retry your request at a later time.
