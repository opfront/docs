+++
date = "2017-06-02T15:44:53-04:00"
title = "Quickstart"
toc = true
weight = 4

+++

## SDK Structure
Before digging deeper into the operations supported by the SDK, we need to introduce the endpoint-independent `Resource` and `Model` classes, which are used by throughout the SDK.
The resource represents a *queryable* and *writeable* collection of items managed by the API, while the model manages an single *editable* and *deletable* entity in our system.

For example, in a typical workflow, you would use a `Resource` provided by the SDK to create, get, and list objects. These operations all return `Model` instances (each of them representing a single API entity), which can then be used to edit and delete that specific entity.

## Ordering a Random Product
Let's say we want to order a random product (*we don't advise you actually do this*).

First, we'll need to create an opfront session. The session handles all requests made to our API and manages your credentials if need be (If you don't have your credentials yet, please contact our [sales team](https://opfront.ca/en/#contact)).
```python
from opfront import OpfrontSession

opfront = OpfrontSession(email='myemail@domain.tld', password='p455w0rd')

```

Once our session is established, we'll need our store's product list in order for us to pick which one we'll order.
Since the `list()` method provided by the SDK returns a generator that handles all the paging logic automatically, we'll need to iterate once
over it to get the complete list of that store's products.

```python
products = opfront.product.list(store_id=42)
product_list = [product for product in products]
```

After getting the complete product list, simply use python's `random.choice()` function to pick one at random.
```python
import random

product_to_order = random.choice(product_list)
```

With our product chosen, we need to prepare our order according to the [order model](/api#order-model).

```python
order_data = {
    'product_ids': [product_to_order.id],
    'client_info': {
        'first_name': 'John',
        'last_name': 'Doe',
        'email': 'john.doe@email.com',
        'address': '1286, Ward Road',
        'phone': '4181234567'
    },
    'store_id': 42,
}
```

Once the order body is defined, we place the order.
```python
order = opfront.order(**order_data)
order.save()
```
