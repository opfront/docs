+++
date = "2017-06-02T16:49:32-04:00"
title = "Session"
toc = true
weight = 6

+++

The `opfront.OpfrontSession()` object is your entry point to the SDK. Upon being instanciated, the session object will log you in using the credentials you provided and will define all resources
that can be accessed via our API.

## Authenticating with the session
It's quite simple to handle your credentials in order for you to perform requests on authenticated API resources.
You only need to pass your credentials to the session constructor.

{{% notice note %}}
At the moment, the python SDK does not support automatic token refresh, which means you'll have to re-create your session object after a period of six hours.
{{% /notice%}}

```python
from opfront import OpfrontSession

# Context manager form
with OpfrontSession(email='myemail@domain.tld', password='p455w0rd') as sess:
    # Your code here
    pass

# Regular form
sess = OpfrontSession(email='myemail@domain.tld', password='p455w0rd')

```

## Available Resources
Head over to our [REST API Documentation](/api) for more details concerning resources.

| Resource  | Endpoint      |        Attribute Name        |
|-----------|---------------|:----------------------------:|
| Banner    | `/banners`    | `OpfrontSession().banner`     |
| Order     | `/orders`     | `OpfrontSession().order`     |
| Product   | `/products`   | `OpfrontSession().product`   |
| Spectacle | `/spectacles` | `OpfrontSession().spectacle` |
| Store     | `/stores`     | `OpfrontSession().store`     |
