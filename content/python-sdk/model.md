+++
date = "2017-06-05T10:11:52-04:00"
title = "API Documentation"
toc = true
weight = 7

+++

## SDK Structure
Before digging deeper into the operations supported by the SDK, we need to introduce the endpoint-independent `Resource` and `Model` classes, which are used by throughout the SDK.
The resource represents a *queryable* and *writeable* collection of items managed by the API, while the model manages an single *editable* and *deletable* entity in our system.

For example, in a typical workflow, you would use a `Resource` provided by the SDK to create, get, and list objects. These operations all return `Model` instances (each of them representing a single API entity), which can then be used to edit and delete that specific entity.

## Resource Operations
#### `OpfrontResource.__call__(self, **kwargs)`

Creates a model instance bound to the resource (endpoint) that created it.

**Args:**

- `kwargs`: Attributes of the model

**Returns:**

- `Model`: Model instance

#### `OpfrontResource.exists(self, res_id)`
Checks if a specific entity exists.

**Args:**

- `res_id`: ID of the entity

**Returns:**

- `bool`: Whether entity exists

#### `OpfrontResource.get(self, res_id)`
Gets an entity instance by ID.

**Args:**

- `res_id` : ID of the entity

**Returns:**

- `Model`: Model instance

#### `OpfrontResource.list(self, page_size=20, **filters)`
Get a generator that iterates lazily over all entities matching the specified filters, taking care of paging automatically.

**Args:**

- `page_size`: Page size to be used for scrolling
- `**filters`: Filters to apply to the collection

**Returns:**

- `Generator<Model>`: Result set

## Model Operations

#### `Model.save(self):`
Updates the model instance (or creates it if it does not yet exist).

**Returns:** Model instance (with updated attributes & generated ID, if applicable)

#### `Model.delete(self):`
Deletes the model instance.
