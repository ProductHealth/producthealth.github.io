---
layout: page
title: "Getting Started"
header: Pages
group: API Docs
permalink: /api/getting-started/index.html
weight: 10
---
{% include JB/setup %}

This page is a step by step guide to get your started with Product Health REST API.

**Note:** The current API has no access control which means that data can be viewed and modified by anybody.

Tools
-----
Product Health API is a REST API.  You can access it programmatically or using one of the suggested following tools:

* [Postman](http://www.getpostman.com/) Chrome extension,
* [Our online API client](/api/try-it-online)

Step 1: Create an organization
------------------------------

The API defines a hierarchy of resources:

    Organizations --> Product Types --> Products --> Channel Data

Organization is at the top level.

To create your organization, execute the following action using your organization name.
Please refere to the [Resource Id](http://localhost:4000/api/reference/general-concepts/#resource_id) section
of the Overview for a list of valid characters for the organization name.


{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations' %}
{% capture request_body %}
{
    "organization": {
        "id": "<your organization id>"
    }
}{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = 'http://phs.io/organizations/&lt;your organization id&gt;' %}
{% include themes/product-health/request-spec.html %}


Step 2: Create a product type
-----------------------------

A product type is a template for products.  You can use it to define the common characteristics of products of the
same model for example.  Product types define

* the list of product properties and
* the list of channels

Let's create a simple product type with only one property and one channel.

Again, you create new resources by issuing a POST to our API entry point:

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/&lt;your organization id&gt;/product_types' %}
{% capture request_body %}
{
    "product_type": {
        "id": "battery-box-model1",
        "schema": {
            "properties": {
                "activated": {
                    "data_type": "boolean",
                    "description": "True if the device is activated",
                    "default_value": true
                }
            },
            "channels": {
                "voltage": {
                    "data_type": "number",
                    "properties": {
                        "units": {
                            "data_type": "string",
                            "description": "SenML units",
                            "default_value": "V"
                        }
                    }
                }
            }
        }
    }
}{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = '/organizations/&lt;your organization id&gt;/product_types/battery-box-model1' %}
{% include themes/product-health/request-spec.html %}


Step 3: Create a product
------------------------

The next step is to create a product belonging to the product type you have just created.
The product id should be a unique in the scope of the product type, .i.e the serial number.

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/&lt;your organization id&gt;/product_types/battery-box-model1/products' %}
{% capture request_body %}
{
    "product":{
        "id": "SN-ABCD-1234",
        "properties":{
            "activated": true
        },
        "channels": {
            "voltage": {
            }
        }
    }
}{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = '/organizations/&lt;your organization id&gt;/product_types/battery-box-model1/products/SN-ABCD-1234' %}
{% include themes/product-health/request-spec.html %}


Step 4: Record data points
--------------------------

After creating your product, you are ready to record some data points.  For example, to record the current voltage a
the value 12.32, just submit the following command:

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/&lt;your organization id&gt;/product_types/battery-box-model1/products/SN-ABCD-1234/channels' %}
{% capture request_body %}
[
    {
        "e":[
            {
                "v":12.32
            }
        ],
        "bn":"voltage"
    }
]{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}

Step 5: Read your data
----------------------

Reading your resources is simply done by issuing a GET request to the same API endpoints that the one you have used to
create them. For example, to get the list of product types, use the following command:

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/&lt;your organization id&gt;/product_types' %}
{% capture response_body %}
[
    {
        "product_type": {
            "id": "battery-box-model1",
            "properties": {
                "name": "Battery Box Model 1"
            },
            "tags": ["battery", "solar"],
            "schema": {
                "properties": {
                    "activated": {
                        "data_type": "boolean",
                        "description": "True if the device is activated",
                        "default_value": true
                    }
                },
                "channels": {
                    "voltage": {
                        "data_type": "number",
                        "properties": {
                            "units": {
                                "data_type": "string",
                                "description": "SenML units",
                                "default_value": "V"
                            }
                        }
                    }
                }
            }
        }
    }
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

In addition, you can get recorded data between a date range using the *start* and *end* request parameters as described
in the (channel reference)[/api/reference/channels#read_historic_records_of_all_channels]

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/&lt;your organization id&gt;/product_types/battery-box-model1/products/SN-ABCD-1234/channels?start=2013-11-20T11:01:46Z&amp;end=2013-11-20T12:01:46Z' %}
{% capture response_body %}
[
    {
        "e":[
            { "v": 12.32, "t":1320067464}
        ],
        "bn":"voltage"
    }
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}


Conclusion
----------
This was a quick overview to get you started.  There is much more you can do with the API like adding more properties and
channels, using tags and recording historical data.  Please refer to the [overview](/api/overview) and
 [reference](/api/reference/general-concepts) for further information.

