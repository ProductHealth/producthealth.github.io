---
layout: page
title: "Product Types"
header: Pages
group: API Docs
permalink: /api/reference/product-types/index.html
weight: 40
---
{% include JB/setup %}

Each Product Type belongs to an organizations.  They group products of the same type together.

The intended use case is to define a product type for each product model.  Product type defines metadata
shared by all products and the product schema.

The product schema allows user to defined the required metadata for any product of a given product type, including
the list of channels.

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
        <td>/organizations/#orgId/product_types</td>
        <td>Read all product types of an organization or create new ones</td>
        <td>GET, POST</td>
    </tr>

    <tr>
        <td>/organizations/#orgId/product_types/#productTypeId</td>
        <td>Read, update or delete a product type</td>
        <td>GET,PUT,DELETE</td>
    </tr>
    </tbody>
</table>


Default Properties
------------------

Organizations have the following attributes which are the default attributes for a metadata resource:

{% include shared/metadata_resource_attributes.html %}

Product Type Attributes
-----------------------

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
        <td>schema</td>
        <td>JSON object</td>
        <td>the schema defining all product belonging to this product type</td>
    </tr>
    </tbody>
</table>

Product Schema
--------------
The product schema is allows user to define product required metadata.  The schema is a JSON object containing
two elements:

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
        <td>the list of product properties</td>
    </tr>
    <tr>
        <td>channels</td>
        <td>JSON object</td>
        <td>the list of product channels</td>
    </tr>
    </tbody>
</table>

###Product properties schema
The product properties schema is a JSON object containing as many name/value pairs are defined product properties,
for example:
{% highlight javaScript %}
{
    "properties":{
        "name": {
            "data_type":"string",
            "description":"This is the name of the device",
            "default_value":null,
        },
        "firmwareVersion": {
            "data_type":"string",
            "description":"This is the current firmware version for this device",
            "default_value":null,
        },
        "batchNumber": {
            "data_type":"number",
            "description":"The batch number when this device was manufactured",
            "default_value":null,
        }
    }
}
{% endhighlight %}


Each property definition is composed of three attributes:

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
        <td>data_type</td>
        <td>string</td>
        <td>the property data type as defined in the main reference document</td>
    </tr>
    <tr>
        <td>description</td>
        <td>string</td>
        <td>a human readable description of the property</td>
    </tr>
    <tr>
        <td>default_value</td>
        <td>a value of the defined type or null</td>
        <td>the default value if the property is not specified when a product is created</td>
    </tr>
    </tbody>
</table>

###Product channels schema
The product channels schema is a JSON object containing as many name/value pairs are defined product channels,
for example:
{% highlight javaScript %}
{
    "channels":{
        "voltage": {
            "data_type":"number",
            "properties":{
                "units": {
                    "data_type":"string",
                    "description":"SenML units",
                    "default_value":"V",
                }
            }
        },
        "current": {
            "data_type":"number",
            "properties":{
                "units": {
                    "data_type":"string",
                    "description":"SenML units",
                    "default_value":"A",
                }
            }
        }
    }
}
{% endhighlight %}


Each channel definition is composed of two attributes:

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
        <td>data_type</td>
        <td>string</td>
        <td>the type of the channel data recored by this channel</td>
    </tr>
    <tr>
        <td>properties</td>
        <td>JSON object</td>
        <td>a definition of the required properties of this channel.
        This object has the same structure as the product properties schema defined above.
        </td>
    </tr>
    </tbody>
</table>




Create a new product type
-------------------------

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/acme/product_types' %}
{% capture request_body %}
{
    "product_type":{
	 "id": "bat-mobile-model1",
        "properties":{
            "name": "Fancy Widget",
            "market": "US",
            "other-property": "other_value",
        },
        "tags":["battery", "solar"],
        "schema":{
            "properties":{
                "name": {
                    "data_type":"string",
                    "description":"This is the name of the device",
                    "default_value":null,
                },
                "firmwareVersion": {
                    "data_type":"string",
                    "description":"This is the current firmware version for this device",
                    "default_value":null,
                },
                "batchNumber": {
                    "data_type":"number",
                    "description":"The batch number when this device was manufactured",
                    "default_value":null,
                }
            },
            "channels":{
                "voltage": {
                    "data_type":"number",
                    "properties":{
                        "units": {
                            "data_type":"string",
                            "description":"SenML units",
                            "default_value":"V",
                        }
                    }
                },
                "current": {
                    "data_type":"number",
                    "properties":{
                        "units": {
                            "data_type":"string",
                            "description":"SenML units",
                            "default_value":"A",
                        }
                    }
                }
            }
        }
    }
}
{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = '/organizations/acme/product_types/bat-mobile-model-1' %}
{% include themes/product-health/request-spec.html %}

Read all product types
----------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types' %}
{% capture response_body %}
[
    {
        "product_type":{
            "id":"bat-mobile-model1",
            "properties":{
                "name":"Fancy Widget",
                "market":"US",
                "other-property":"other_value",
            },
            "tags":["battery", "solar"]
        },
    },
    ...
]
{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}

Read a product type
-------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1' %}
{% capture response_body %}
{
    "product_type":{
        "id":"bat-mobile-model1",
        "properties":{
            "name":"Fancy Widget",
            "market":"US",
            "other-property":"other_value",
        },
        "tags":["battery", "solar"]
    },
}{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}

Update a product type
---------------------

{% assign request_method = 'PUT' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1' %}
{% capture request_body %}
{
    "product_type":{
        "id":"bat-mobile-model1",
        "properties":{
            "name":"Fancy Widget",
            "market":"US",
            "other-property":"other_value",
        },
        "tags":["battery", "solar"]
    },
}{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}

Delete a product type
---------------------

{% assign request_method = 'DELETE' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1' %}
{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}