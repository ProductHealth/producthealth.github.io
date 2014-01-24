---
layout: page
title: "Products"
header: Pages
group: API Docs
permalink: /api/reference/products/index.html
weight: 50
---
{% include JB/setup %}

Each product belongs to a product type.  They correspond to the "thing" that is monitored and controlled.

Each product has metadata for the product itself and for the channels.  These metadata must correspond to the
schema defined in the product type.

Requests
--------

<table class="content">
    <thead>
    <tr>
        <th><strong>URL</strong></th>
        <th><strong>Description</strong></th>
        <th><strong>Methods</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>/organizations/#orgId/product_types/#productTypeId/products</td>
        <td>Read all product of a given product type or create new ones</td>
        <td>GET, POST</td>
    </tr>

    <tr>
        <td>/organizations/#orgId/product_types/#productTypeId/products/#productId</td>
        <td>Read, update or delete a product</td>
        <td>GET,PUT,DELETE</td>
    </tr>
    </tbody>
</table>


Default Properties
------------------

Organizations have the following attributes which are the default attributes for a metadata resource:

{% include shared/metadata_resource_attributes.html %}

Product Attributes
------------------

<table class="content">
    <thead>
    <tr>
        <th><strong>Name</strong></th>
        <th><strong>Type</strong></th>
        <th><strong>Description</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>channels</td>
        <td>JSON object</td>
        <td>the channels metadata</td>
    </tr>
    </tbody>
</table>

Product Channels
----------------
The product channels attribute gives the channel metadata.  It is a JSON object containing
as many key/value pairs as available channels, for example:
{% highlight javaScript %}
{
    "channels":{
        "voltage":{
            "properties":{
                "units":"minivolts"
            },
            "tags":["drifting"],
        },
        "current":{
            "properties":{
                "units":"amps"
            },
            "tags":["stable"],
        },
    }
}
{% endhighlight %}

Each property definition is composed of two attributes:

<table class="content">
    <thead>
    <tr>
        <th><strong>Name</strong></th>
        <th><strong>Type</strong></th>
        <th><strong>Description</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>properties</td>
        <td>JSON object</td>
        <td>the channel properties</td>
    </tr>
    <tr>
        <td>tags</td>
        <td>JSON array</td>
        <td>the list of channel tags</td>
    </tr>
    </tbody>
</table>

Create a new product
--------------------

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model-1' %}
{% capture request_body %}
{
    "product":{
        "id": "SN-ABCD-1234",
        "properties":{
            "name": "my device",
            "firmwareVersion": "1.0.1a",
            "batchNumber": 6
        },
        "tags":["prototype"],
        "channels": {
            "voltage": {
                "properties": {
                    "units":"minivolts"
                },
                "tags": ["drifting"],
            }
        }
    }
}{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = '/organizations/acme/product_types/bat-mobile-model-1/products/SN-ABCD-1234' %}
{% include themes/product-health/request-spec.html %}

Read all products
----------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model-1/products' %}
{% capture response_body %}
[
    {
        "product":{
            "id":"SN-ABC-1234",
            "properties":{
                "name":"my device",
                "firmwareVersion":"1.0.1a",
                "batchNumber":6
            },
            "channels":{
                "voltage":{
                    "properties":{
                        "units":"minivolts"
                    },
                    "tags":["drifting"],
                },
                "current":{
                    "properties":{
                        "units":"amps"
                    },
                    "tags":["stable"],
                },
            },
            "tags":["prototype"]
        },
    },
    ...
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}

Read a product
--------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234' %}
{% capture response_body %}
{
    "product":{
        "id":"SN-ABC-1234",
        "properties":{
            "name":"my device",
            "firmwareVersion":"1.0.1a",
            "batchNumber":6
        },
        "channels":{
            "voltage":{
                "properties":{
                    "units":"minivolts"
                },
                "tags":["drifting"],
            },
            "current":{
                "properties":{
                    "units":"amps"
                },
                "tags":["stable"],
            },
        },
        "tags":["prototype"]
    },
}{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}

Update a product
----------------

{% assign request_method = 'PUT' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234' %}
{% capture request_body %}
{
    "product":{
        "properties":{
            "name": "my device with an updated name",
            "firmwareVersion": "1.0.1b",
            "version": 3
        },
        "tags":["healthy"],
        "channels":{
            "voltage": {
                "properties": {
                    "units":"minivolts"
                },
                "tags": ["drifting"],
            },
            "current": {
                "properties": {
                    "units":"amps"
                },
                "tags": ["stable"],
            }
        }
    }
}{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}

Delete a product
----------------

{% assign request_method = 'DELETE' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234' %}
{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}