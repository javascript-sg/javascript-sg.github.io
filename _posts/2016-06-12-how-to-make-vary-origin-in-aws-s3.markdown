---
published: true
title: How to make Vary Origin in AWS S3
layout: post
---
I've been trying to make Vary Origin work with Amazon Web Services S3. Here is how.

You need to Add CORS Configuration. Paste in the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <CORSRule>
    <AllowedOrigin>http*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
  </CORSRule>
  <CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
  </CORSRule>
</CORSConfiguration>
```

<h3>How to test</h3>

Open the terminal or command line and using curl:

```sh
curl -sI \
  -H "Origin: https://kw.sg" \
  -H "Access-Control-Request-Method: GET" \
  https://s3.amazonaws.com/animate-vpaid-bridge/sample-1.xml
```

I sent the origin as https://kw.sg

Notice that in the response, `Access-Control-Allow-Origin` is `https://kw.sg`:

```
HTTP/1.1 200 OK
x-amz-id-2: 0BcMkyAeoPt/w/oevI+qtn1ihpODlDkxkqKlVg76L+OLxzPM1mxLXjRhbU3ZkQw7esOrtpFDaVU=
x-amz-request-id: 8AFE776F01F4A04B
Date: Wed, 10 Aug 2016 21:09:46 GMT
Access-Control-Allow-Origin: https://kw.sg
Access-Control-Allow-Methods: GET
Access-Control-Max-Age: 3000
Access-Control-Allow-Credentials: true
Vary: Origin, Access-Control-Request-Headers, Access-Control-Request-Method
Last-Modified: Thu, 18 Feb 2016 02:01:48 GMT
ETag: "826700c1df4af9fea1aea4e7b36035e7"
Accept-Ranges: bytes
Content-Type: text/xml
Content-Length: 1888
Server: AmazonS3
```

Now lets try sending only the Origin without `Access-Control-Request-Method`:

```sh
curl -sI \
  -H "Origin: https://kw.sg" \
  https://s3.amazonaws.com/animate-vpaid-bridge/sample-1.xml
```

Notice that in the response, `Access-Control-Allow-Origin` is `*`:

```
HTTP/1.1 200 OK
x-amz-id-2: yjG+iEUBzesZscpgzU9qwAplYzTBoX2oLty9zPCEybE90BYF8oJDXZ03XaqvGcIq0QU2qU19b18=
x-amz-request-id: 7BC3744AC964A9F6
Date: Wed, 10 Aug 2016 21:39:43 GMT
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, HEAD
Vary: Origin, Access-Control-Request-Headers, Access-Control-Request-Method
Last-Modified: Thu, 18 Feb 2016 02:01:48 GMT
ETag: "826700c1df4af9fea1aea4e7b36035e7"
Accept-Ranges: bytes
Content-Type: text/xml
Content-Length: 1888
Server: AmazonS3
```

Note the above where I am able to get the Access-Control-Allow-Origin to return https://kw.sg as well.