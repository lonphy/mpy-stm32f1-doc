---
layout: post
title:  "MicroPython ussl - SSL/TLS库"
date:   2019-08-18 0:0:0 +0000
category: library
---

"ussl" -- SSL/TLS module
************************

*This module implements a subset of the corresponding* "CPython"
*module, as described below. For more information, refer to the
original CPython documentation:* "ssl".

This module provides access to Transport Layer Security (previously
and widely known as “Secure Sockets Layer”) encryption and peer
authentication facilities for network sockets, both client-side and
server-side.


Functions
=========

ussl.wrap_socket(sock, server_side=False, keyfile=None, certfile=None, cert_reqs=CERT_NONE, ca_certs=None)

   Takes a "stream" *sock* (usually usocket.socket instance of
   "SOCK_STREAM" type), and returns an instance of ssl.SSLSocket,
   which wraps the underlying stream in an SSL context. Returned
   object has the usual "stream" interface methods like "read()",
   "write()", etc. In MicroPython, the returned object does not expose
   socket interface and methods like "recv()", "send()". In
   particular, a server-side SSL socket should be created from a
   normal socket returned from "accept()" on a non-SSL listening
   server socket.

   Depending on the underlying module implementation in a particular
   "MicroPython port", some or all keyword arguments above may be not
   supported.

Warning: Some implementations of "ussl" module do NOT validate
  server certificates, which makes an SSL connection established prone
  to man-in-the-middle attacks.


Exceptions
==========

ssl.SSLError

   This exception does NOT exist. Instead its base class, OSError, is
   used.


Constants
=========

ussl.CERT_NONE
ussl.CERT_OPTIONAL
ussl.CERT_REQUIRED

   Supported values for *cert_reqs* parameter.
