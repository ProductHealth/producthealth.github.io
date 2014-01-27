---
layout: page
title: "Organizations"
header: Pages
group: API Docs
permalink: /api/reference/organizations/index.html
weight: 30
---
{% include JB/setup %}


Organization is the metadata resource at the top level of the resource hierarchy.
Organizations allow multi-tenancy of the service.

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
        <td>/organizations</td>
        <td>Read all organizations or create new ones</td>
        <td>GET, POST</td>
    </tr>

    <tr>
        <td>/organizations/#orgId</td>
        <td>Read, update or delete an organization</td>
        <td>GET,PUT,DELETE</td>
    </tr>
    </tbody>
</table>


Default Properties
------------------

Organizations have the following attributes which are the default attributes for a metadata resource:

{% include shared/metadata_resource_attributes.html %}



Create a new organization
-------------------------

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations' %}
{% capture request_body %}
{
    "organization": {
        "id": "acme",
        "properties":{
            "country": "UK"
        },
        "tags":["tag1", "tag2"]
    }
}{% endcapture %}

{% assign response_status = '201 Created' %}
{% assign location_header = 'http://phs.io/organizations/acme' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Read all organizations
-------------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations' %}
{% capture response_body %}
[
    {
        "organization":{
            "id":"acme",
            "properties":{
                "a-property":"aValue",
                "other-property":"other_value"
            }
        }
    },
    {
        "organization":{
            "id":"acme2",
            "properties":{
                "a-property":"aValue",
                "other-property":"other_value"
            }
        }
    }
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Read an organization
--------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/#orgId' %}
{% capture response_body %}
{
    "organization":{
        "id":"acme",
        "properties":{
            "a-property":"aValue",
            "other-property":"other_value"
        }
    }
}{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Update an organization
----------------------

{% assign request_method = 'PUT' %}
{% assign request_endpoint = '/organizations/#orgId' %}
{% capture request_body %}
{
    "organization":{
        "properties":{
            "a-property":"aValue",
            "other-property":"other_value"
        }
    }
}{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Delete an organization
----------------------

{% assign request_method = 'DELETE' %}
{% assign request_endpoint = '/organizations/#orgId' %}
{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}