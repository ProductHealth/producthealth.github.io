---
layout: page
title: "Channels"
header: Pages
group: API Docs
permalink: /api/reference/channels/index.html
weight: 60
---
{% include JB/setup %}


A channel holds time-series channel data.  Every product measurements are recorded under a channel.
Channel data follow the [SenML](http://tools.ietf.org/html/draft-jennings-senml-08) specification.


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
        <td>/organizations/#orgId/product_types/#productTypeId/products/#productId/channels</td>
        <td>Record and read channel data</td>
        <td>GET, POST</td>
    </tr>
    </tbody>
</table>


Recording instant values on multiple channels
---------------------------------------------

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234/channels' %}
{% capture request_body %}
[
    {
        "e":[
            {
                "v":12.32
            }
        ],
        "bn":"voltage"
    },
    {
        "e":[
            {
                "v":3.45
            }
        ]
        ,
        "bn":"current"
    },
    {
        "e":[
            {
                "bv":true
            }
        ]
        ,
        "bn":"active"
    },
    {
        "e":[
            {
                "sv":"recording"
            }
        ]
        ,
        "bn":"state"
    }
]{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Recording historic values on multiple channels
----------------------------------------------

When recording historic data, you have to specifying the full timestamp on each record or a base time like this:

{% assign request_method = 'POST' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234/channels' %}
{% capture request_body %}
[
    {
        "e":[
            { "v": 12.32, "t":1320067464},
            { "v": 12.33, "t":1320067465},
            { "v": 12.34, "t":1320067466}
        ],
        "bn":"voltage"
    },
    {
        "e":[
            { "v": 0.452, "t":1320067464},
            { "v": 0.453, "t":1320067465},
            { "v": 0.454, "t":1320067466}
        ],
        "bn":"current"
    }
]{% endcapture %}

{% assign response_status = '204 No Content' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Read last record of all channels
--------------------------------

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234/channels' %}
{% capture response_body %}
[
    {
        "e":[
            { "v": 12.32, "t":1320067464}
        ],
        "bn":"voltage"
    },
    {
        "e":[
            { "v": 0.452, "t":1320057431}
        ],
        "bn":"current"
    }
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}

Read historic records of all channels
-------------------------------------

To read historical data, at least one of the following request parameters must be used:

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
        <td>start</td>
        <td><a href="http://en.wikipedia.org/wiki/ISO_8601">ISO8601</a> string</td>
        <td>the start time</td>
    </tr>
    <tr>
        <td>end</td>
        <td><a href="http://en.wikipedia.org/wiki/ISO_8601">ISO8601</a> string</td>
        <td>the end time</td>
    </tr>
    </tbody>
</table>

If only one of the parameter is given, the following apply:

* if the start time is not given, all records up to the end time will be returned,
* if the end time is not given, all record from the start time until now will be returned.

{% assign request_method = 'GET' %}
{% assign request_endpoint = '/organizations/acme/product_types/bat-mobile-model1/products/SN-ABCD-1234/channels?start=2013-11-20T11:01:46Z&amp;end=2013-11-20T12:01:46Z' %}
{% capture response_body %}
[
    {
        "e":[
            { "v": 12.32, "t":1320067464},
            { "v": 12.33, "t":1320067465},
            { "v": 12.34, "t":1320067466}
        ],
        "bn":"voltage"
    },
    {
        "e":[
            { "v": 0.452, "t":1320067464},
            { "v": 0.453, "t":1320067465},
            { "v": 0.454, "t":1320067466}

        ],
        "bn":"current"
    }
]{% endcapture %}

{% assign response_status = '200 OK' %}
{% include themes/product-health/request-spec.html %}
{% include themes/product-health/response-spec.html %}