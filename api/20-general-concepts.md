---
layout: page
title: "General Concepts"
header: Pages
group: API Docs
permalink: /api/reference/general-concepts/index.html
weight: 20
---
{% include JB/setup %}


Service Entry Point
-------------------

We collect and access product data through our REST API located at

    http://phs.io/v1

All path defined in this document are relative to this entry point.

Resources
---------

We define the following resources organized hierarchically:

    Organizations --> Product Types --> Products --> Channel Data


There are two types of data in the system; metadata and time-series data:

**Metadata** is information about a product or a channel of data that does not
change very regularly (or in many cases, at all). This might include things like the version of the
firmware, the product batch number or the geolocation. The metadata for a product is user-defined.

**Time-series** data is information about a product that can change many times per second.
This might include things like the battery voltage and current and temperature.
The time-series data is stored in “channels”.  Channels can have their own metadata.

Here is a summary of each resource:

<table class="content">
    <thead>
    <tr>
        <th><strong>Resource</strong></th>
        <th><strong>Description</strong></th>
        <th><strong>Type</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>Organizations</td>
        <td>Organizations are the top level resources and allow multi-tenancy of the service.
            This endpoint allows the API user to define organizations under which product_types and products
            are defined.
        </td>
        <td>Metadata</td>
    </tr>

    <tr>
        <td>Product Types</td>
        <td>This endpoint allows the API user to define a schema for their product, including the
            metadata and time-series data for the device. It allows the user to specify the attributes for the product.
        </td>
        <td>Metadata</td>
    </tr>
    <tr>
        <td>Products</td>
        <td>Once a product type has been created, instances of it can be created using this endpoint.
            Each product is a container of metadata about the product and its channels.
        </td>
        <td>Metadata</td>
    </tr>
    <tr>
        <td>Channel Data</td>
        <td>Channel Data are holder of time series data. Time-series data are represented
            using <a href="http://tools.ietf.org/html/draft-jennings-senml-08">SenML</a>.
        </td>
        <td>Time-series</td>
    </tr>
    </tbody>
</table>

Resource Id
-----------

A resource has public id that must be unique in their respective scope.

Resource id MUST consist only of characters out of the set "A" to "Z", "a" to "z", "0" to "9", "-", ":", ".", or "_" and
it MUST start with a character out of the set "A" to "Z", "a" to "z", or "0" to "9".  This restricted character set was
chosen so that these names can be directly used as in other types of URI including segments of an HTTP path with no
special encoding.  Organization id must be unique at service level, product type id must be unique inside an
organization, product id must be unique inside a given product type and channel id must be unique in a given product.


URL Scheme
----------

The URL scheme is nested and has the following structure:

    /organizations/<org-id>/product_types/<type-id>/products/<prod-id>/channels

This scheme allows user to manage resource using REST principles and easily explore available resources.

Data Types
----------

Resource properties and channel data have types based on a subset of the [JSON](http://json.org/) types:

* Resource properties: string, number, boolean, null plus a timestamp type,
* Record data: string, number, boolean.

<table class="content">
  <thead>
    <tr>
        <th><strong>Name</strong></th>
        <th><strong>SenML Tag</strong></th>
        <th><strong>Example</strong></th>
        <th><strong>Description</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>string</td>
        <td>sv</td>
        <td>&quot;abcDEF&eacute;@&quot;</td>
        <td>UTF-8 encoded string</td>
    </tr>
    <tr>
        <td>number</td>
        <td>v</td>
        <td>
            123<br/>
            10.4e2<br/>
            -0.23
        </td>
        <td>positive and negative integer and floating point numbers</td>
    </tr>
    <tr>
        <td>boolean</td>
        <td>bv</td>
        <td>
            true<br/>
            false
        </td>
        <td>Boolean values</td>
    </tr>
    <tr>
        <td>timestamp</td>
        <td>t</td>
        <td>
            123456.000<br/>
            3223432.02<br/>
            23456.001<br/>
            -345456
        </td>
        <td>Positive values represent the number of second since the
            [Unix Epoch](http://en.wikipedia.org/wiki/Unix_time), negative values represent
            number of second in the past from ‘now’, zero means ‘now’.  Precision is up to the millisecond using
            decimal notation.  0.001 is one millisecond.  Digits from the fourth digit are truncated.  No rounding
            is taking place.</td>
    </tr>
    <tr>
        <td>null</td>
        <td>N/A</td>
        <td>null</td>
        <td>null is its own type and can be assigned to any type.</td>
    </tr>
  </tbody>
</table>

For channel data number, a “units” property should be added as metadata.
This “units” property should contain the units as defined in
[SenML Units Registry](http://tools.ietf.org/html/draft-jennings-senml-08#section-10.1).

Status Codes
------------

We only return a subset of the HTTP status codes defined in
[RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html):

<table class="content">
    <thead>
    <tr>
        <th><strong>Code</strong></th>
        <th><strong>Message</strong></th>
        <th><strong>Header</strong></th>
        <th><strong>Body</strong></th>
        <th><strong>Usage</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>200</td>
        <td>OK</td>
        <td></td>
        <td>JSON object or array</td>
        <td>GET Success</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Created</td>
        <td>Location</td>
        <td>Empty</td>
        <td>POST Success</td>
    </tr>
    <tr>
        <td>204</td>
        <td>No Content</td>
        <td></td>
        <td>Empty</td>
        <td>PUT and DELETE success</td>
    </tr>
    <tr>
        <td>400</td>
        <td>Bad Request</td>
        <td></td>
        <td>JSON Error Object</td>
        <td>GET, POST, PUT, DELETE: server cannot parse the request</td>
    </tr>
    <tr>
        <td>401</td>
        <td>Unauthorized</td>
        <td></td>
        <td>JSON Error Object</td>
        <td>GET, POST, PUT, DELETE: not enough access rights</td>
    </tr>
    <tr>
        <td>403</td>
        <td>Forbidden</td>
        <td></td>
        <td>JSON Error Object</td>
        <td>POST, PUT validation error</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Not Found</td>
        <td></td>
        <td>JSON Error Object</td>
        <td>GET, POST, PUT, DELETE of unknown resource</td>
    </tr>
    <tr>
        <td>405</td>
        <td>Method Not Allowed</td>
        <td></td>
        <td>HTML Error Page</td>
        <td>Method not implemented for the given resource</td>
    </tr>
    <tr>
        <td>500</td>
        <td>Internal Server Error</td>
        <td></td>
        <td>JSON Error Object</td>
        <td>The server has encountered an error</td>
    </tr>
    </tbody>
</table>

Error Object
------------

An error object is returned in the response body when an error occurs.  The error object is a JSON object like
the following:

{% highlight javaScript %}
{
    "error_status": {
        "status": 404,
        "message": "Not Found",
        "details": ["Organization foobar does not exist"],
        "more_info": "http://dev.producthealth.com/api"
    }
}
{% endhighlight %}

The fields of this error object are:

<table class="content">
    <thead>
    <tr>
        <th><strong>Name</strong></th>
        <th><strong>Type</strong></th>
        <th><strong>Mandatory</strong></th>
        <th><strong>Description</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>status</td>
        <td>number</td>
        <td>yes</td>
        <td>The HTTP status conde</td>
    </tr>
    <tr>
        <td>message</td>
        <td>string</td>
        <td>yes</td>
        <td>The HTTP error message associated with the code</td>
    </tr>
    <tr>
        <td>details</td>
        <td>array of strings</td>
        <td>no</td>
        <td>An array of human readable error messages</td>
    </tr>
    <tr>
        <td>more_info</td>
        <td>string</td>
        <td>no</td>
        <td>Any addition information that would allow a human to correct the error</td>
    </tr>
    </tbody>
</table>


Common Resource Characteristics
-------------------------------

###Metadata Resource Attributes

Metadata resources, i.e. Organizations, Product Types, Products and Channels share some attributes:

{% include shared/metadata_resource_attributes.html %}

###JSON representation

Resource JSON representation is an JSON object with only one key: the resource type:

<table class="content">
    <thead>
    <tr>
        <th><strong>Resource</strong></th>
        <th><strong>Type Key</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>Organization</td>
        <td>organization</td>
    </tr>

    <tr>
        <td>Product Type</td>
        <td>product_type</td>
    </tr>
    <tr>
        <td>Product</td>
        <td>Product</td>
    </tr>
    <tr>
        <td>Channel Data</td>
        <td>e</td>
    </tr>
    </tbody>
</table>

The simplest metadata resource is the following:

{% highlight javaScript %}
{
    "organization": {
        "id": "an_id",
        "properties": {},
        "tags": []
    }
}
{% endhighlight %}

The simplest channel data resource is the following:

{% highlight javaScript %}
{"e":[{ "v":23.5 }]}
{% endhighlight %}