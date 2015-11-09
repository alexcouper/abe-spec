[![Build Status](http://img.shields.io/travis/apibyexample/abe-spec/master.svg)](https://travis-ci.org/apibyexample/abe-spec)
[![devDependency Status](https://david-dm.org/apibyexample/abe-spec/dev-status.svg)](https://david-dm.org/apibyexample/abe-spec#info=devDependencies)

API by Example
==============

API by Example (ABE) is a format that can be used as a contract between services, describe
APIs in a format that can be programmatically consumed by tools.

The main idea behind it is to illustrate how an API works via concrete
examples, that can be used to generate tests, stubs and documentation for a
given API.

API by Example was born out of the work its initial authors have been
doing at Rockabox, while developing better ways to test API producers and
consumers.

The format is initially limited to HTTP-based APIs, and optimised for (but not
limited to) APIs that exchange JSON data.

Specification
-------------

The main building block of ABE is a JSON file that illustrates an API endpoint.
One API call is an HTTP request that consists of a URL (pattern), sample
request data and sample response data. Additional fields are included for
documentation purposes.

A schema describing the ABE format using [JSON Schema](http://json-schema.org) can be found in
[schema.json](schema.json)

The skeleton of an ABE file is:

```json
{
    "description": "<description>",
    "url": "<url>",
    "method": "<http method>",
    "examples": {
        "<label>": {
            "description": "<description>",
            "request": {
                "queryParams": <params>,
                "headers": <headers>,
                "body": <body>,
                "url": <url>,
                "method": <http method>
            },
            "response": {
                "status": <status>,
                "headers": <headers>,
                "body": <body>
            }
        }
    }
}
```

* `description` is an optional text describing the API or the concrete example
* `url` is a base location for our API. It is inherited by each example
  if declared at the top, but an examples definition (if present) takes
  precedence.
* `examples` contains your different API examples for the contract, this can be
  in the form of an `array` or an `object`.
* `label` is an arbitrary label that you can use to refer to one concrete
  example, useful when you want to include more than one possible responses.
  For instance, `"OK"` and `"Not found"`, or `"Empty"` versus `"One"`
  versus `"Many"`.
* `http method` is one of the HTTP verbs `GET`, `POST`, `PUT`... It is
  inherited by each example if declared at the top, but an examples definition
  (if present) takes precedence.
* `params` are query string parameters to add to the URL
* `headers` is an optional object mapping headers to values
* `status` is HTTP status: `200`, `404`, etc...
* `body` is the payload, in JSON format, or a properly escaped string.
  Binary encodings are not supported at the moment.


To illustrate with a concrete example:

```json
{
    "description": "A list of brands",
    "url": "/campaigns/brands/",
    "method": "GET",
    "examples": {
        "Fetch-OK": {
            "description": "Collection found",
            "request": {
                "queryParams": {},
                "body": {}
            },
            "response": {
                "status": 200,
                "body": [
                    {
                        "id": 1,
                        "name": "Microsoft"
                    },
                    {
                        "id": 2,
                        "name": "Monsoon"
                    },
                    {
                        "id": 3,
                        "name": "Mars"
                    },
                    {
                        "id": 4,
                        "name": "John Lewis"
                    }
                ]
            }
        },
        "Create-OK": {
            "description": "Creation of brands is legit",
            "request": {
                "method": "POST",
                "queryParams": {},
                "body": {
                    "name": "Nike"
                }
            },
            "response": {
                "status": 200,
                "body": [
                    {
                        "id": 1,
                        "name": "Microsoft"
                    },
                    {
                        "id": 2,
                        "name": "Monsoon"
                    },
                    {
                        "id": 3,
                        "name": "Mars"
                    },
                    {
                        "id": 4,
                        "name": "John Lewis"
                    },
                    {
                        "id": 5,
                        "name": "Nike"
                    }
                ]
            }
        }
    }
}
```
