---
layout: page
title: "CBOR"
header: Pages
group: API Docs
permalink: /api/reference/cbor/index.html
weight: 30
---
{% include JB/setup %}


The API currently understand [JSON](http://json.org) and [CBOR](http://cbor.io), JSON being the default type.


CBOR
----

CBOR is a binary representation with a logical structure similar to JSON but more compact.
The MIME type for CBOR is **application/cbor**.

To use CBOR when reading data, just add the header:

    Accept: application/cbor

To send data using the CBOR format, just add the header:

    Content-Type: application/cbor


Limitation
----------

The API currently does not interpret and generate Decimal Fractions as defined in section
[2.4.3](http://tools.ietf.org/html/rfc7049#section-2.4.3) of the RFC.


Tool
----

Because CBOR is a binary format, it can be difficult to read or generate.  The [cbor.me](http://cbor.me) website will
certainly help you.