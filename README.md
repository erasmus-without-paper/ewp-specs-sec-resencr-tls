TLS and Response Confidentiality
================================

This document describes how to accomplish confidentiality of EWP HTTP responses
with the use of TLS transport.

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Introduction
------------

This is a pretty standard method of achieving HTTP response confidentiality. It
simply requires TLS (HTTPS) to be used. Is assumes that such responses cannot
be overheard, and **it doesn't apply any additional response body encryption**.


Implementing a server
---------------------

You MUST acquire a CA-signed certificate for your domain(s), and install it on
your servers. These are just the "regular" domain-validated certificates. You
MUST serve your endpoint(s) via HTTPS.


Implementing a client
---------------------

Not much more to describe here. You simply MUST make your requests via HTTPS
protocol.


Security considerations
-----------------------

### When *not* to use this method

In case of the server:

 * If the response MAY contain private user data, and you **don't trust your
   own internal network** (the one between your actual server and your TLS
   terminator), then you MUST NOT include `<tls/>` in your
   `<response-encryption-methods>`.

In case of the client:

 * If the response MAY contain private user data, and you cannot be sure that
   your root/intermediate CA certificate store has not been tampered with,
   then you MUST NOT use this method.


### Main security questions

The [Authentication and Security][sec-intro] document
[requires][sec-method-rules] each response encryption method to explicitly
answer a couple of questions:

> How the client's request must look like? How can the server know, that the
> client *wants the server* to use this particular method of encryption?

The client simply makes its request via HTTPS. It the request doesn't include
any indication that some other server authentication method is desired, then
the server may assume that the client uses this one.

> How the client delivers his encryption key to the server?

The client uses HTTPS server certificate validation to make sure that it is
communicating with the proper party. The server receives required encryption
keys during the TLS handshake.

Note, that this only works when the clients can trust their CA certificate
stores. See *When not to use this method* chapter above.

> How to encrypt and decrypt the response? Which parts are covered by the
> encryption and which are not?

HTTPS does that on its own. All parts of the response are covered, including
all headers.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/stable-v1/README.md#statuses
[sec-intro]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro
[sec-method-rules]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro#rules
