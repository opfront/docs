+++
date = "2017-06-05T10:11:52-04:00"
title = "API Documentation"
toc = true
weight = 7

+++
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
{{% notice note %}}
Since the model performs an additional HTTP request to check if the instance already exists,
you can also force creation using `OpfrontResource().create(Model)` or force update using `OpfrontResource().update(Model)`.
{{% /notice%}}

**Returns:** Model instance (with updated attributes & generated ID, if applicable)

#### `Model.delete(self):`
Deletes the model instance.
