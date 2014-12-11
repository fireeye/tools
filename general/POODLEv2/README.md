This is a linux build of OpenSSL 1.0.1j with modified TLS padding to test for POODLE like vulnerabilities in TLS servers.
For details of the changes made, see here:

https://github.com/jamesoc-feye/openssl/commit/bd769939c3c7b51df8eed02a79c675701a6be3bd

Example Usage: 
```
(printf 'GET / HTTP/1.1\r\nHost: localhost\r\n\r\n'; sleep 5) | ./openssl_bad_tls_padding s_client -tls1 -cipher DES-CBC3-SHA -connect 127.0.0.1:443 2>&1 |less
```

Ensure you're using a suitable cipher such as DES-CBC3-SHA which uses TLS padding and not AEAD

A NON-vulnerable server will look something like:
```
140026524681896:error:140943FC:SSL routines:SSL3_READ_BYTES:sslv3 alert bad record mac:s3_pkt.c:1275:SSL alert number 20
140026524681896:error:1409E0E5:SSL routines:SSL3_WRITE_BYTES:ssl handshake failure:s3_pkt.c:598:
```

A vulnerable server will return a HTTP response, such as:
```
HTTP/1.0 200 OK
Server: Generic Web Server
Date: Tue, 26 Aug 1997 22:10:05 GMT
Content-type: text/plain

```
