gotls
=====

This repository is a copy/fork of the Go 1.4.2 crypto/tls directory with small
changes to implement session caching based on session IDs.

Go 1.4 TLS implementation supports SSL session resumption using session tickets
which involve the server encrypting the state but having the clients store it
and then present it in the next request.  If the server can decrypt the
presented blob it will use the cached parameters inside and resume the session.
In order to do this securely and across machines requires a key distribution
mechanism.  In addition, tickets are a TLS extension and many client libraries
do not support them.  Session ID caching store the state on the server and can
be implemented with any distributed key-value store (e.g. memcache or Redis).
See the companion module sslsessionpool (github.com/elorimer/sslsessionpool)
for an implmentation of a backend that uses memcache.

Upstream is not particularly interested in supporting session IDs
(https://groups.google.com/forum/#!topic/golang-dev/ClGuwk-n_L4) but I needed
them and perhaps this will be useful for someone else.

One should be very cautious when replacing the standard crypto library with
something from Github so feel free to diff the files with the standard Go
library directory.  Only two files need changes - common.go and
handshake_server.go.  The changes should be straight-forward and can be made
manually if that's safer.