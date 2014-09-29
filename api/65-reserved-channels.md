---
layout: page
title: "Reserved Channels"
header: Pages
group: API Docs
permalink: /api/reference/reserved-channels/index.html
weight: 60
---
{% include JB/setup %}

This document defines reserved channels that have a conventional semantic.

<table class="content">
    <thead>
    <tr>
        <th><strong>Channel Name</strong></th>
        <th><strong>Description</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>position</td>
        <td>Product location coordinates</td>
    </tr>

    <tr>
        <td>position_error_radius</td>
        <td>Error radius on the position position</td>
    </tr>
    </tbody>
</table>


Position
--------
The *position* channel is used to store product location coordinates.  The channel holds a string
formatted as following:

{% highlight javaScript %}
"41.40338, 2.17403"
{% endhighlight %}

which is a coma separated latitude and longitude (latitude first) formatted as decimal degrees as described here:
[http://en.wikipedia.org/wiki/Decimal_degrees](http://en.wikipedia.org/wiki/Decimal_degrees).  The degree symbols are
omitted.


The corresponding product type schema is the following:

{% highlight javaScript %}
{
    "channels":{
        "position": {
            "data_type":"string",
            "properties":{
                "units": {
                    "data_type":"string",
                    "description":"Geo coordinates in decimal degrees",
                    "default_value":"geo"
                }
            }
        }
    }
}{% endhighlight %}


Position Error Radius
---------------------

The *position_error_radius* is a channel used to record the error radius on the position position channel in meter.
In other word, the position_error_radius is a number representing the error radius in meter.

The corresponding product type schema is the following:

{% highlight javaScript %}
{
    "channels":{
        "position_error_radius": {
            "data_type":"number",
            "properties":{
                "units": {
                    "data_type":"string",
                    "description":"SenML units",
                    "default_value":"m"
                }
            }
        }
    }
}{% endhighlight %}