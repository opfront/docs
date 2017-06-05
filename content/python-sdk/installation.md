+++
date = "2017-06-02T15:42:04-04:00"
title = "Installation"
toc = true
weight = 1

+++

### Installing with pip
Installing with pip is pretty straightforward, as our SDK has very few dependencies.
{{% notice note %}}
For the moment, our SDK only supports Python 3.x .
{{% /notice%}}
```shell
$ pip install opfront
```

### Installing manually

#### Installing the package
```shell
$ git clone git@github.com:opfront/python-sdk.git

$ cd python-sdk/

$ python setup.py install
```

#### Running the tests
```shell
$ python -m unittest discover -v -s .
```
