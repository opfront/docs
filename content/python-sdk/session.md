+++
date = "2017-06-02T16:49:32-04:00"
title = "Session"
toc = true
weight = 6

+++

The `opfront.OpfrontSession()` object is your entry point to the SDK. Upon being instanciated, the session object will log you in using the credentials you provided and will define all resources
that can be accessed via our API.

TODO: More details here

## Available Resources
Head over to our [REST API Documentation](/api) for more details concerning resources.

| Resource  | Endpoint      |        Attribute Name        |
|-----------|---------------|:----------------------------:|
| Order     | `/orders`     | `OpfrontSession().order`     |
| Product   | `/products`   | `OpfrontSession().product`   |
| Spectacle | `/spectacles` | `OpfrontSession().spectacle` |
| Store     | `/stores`     | `OpfrontSession().store`     |
