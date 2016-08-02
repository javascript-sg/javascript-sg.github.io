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
        <AllowedOrigin>http://*</AllowedOrigin>
        <AllowedOrigin>https://*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

<h3>How to test</h3>

Open the terminal or command line and using curl:

```sh
curl -sI \
-H "Origin: https://kw.sg" \
-H "Access-Control-Request-Method: GET" \
https://s3.amazonaws.com/m-vast-vpaid-server/static/vast/vpaid-in-vpaid.xml
```

I sent the origin as https://kw.sg

You should get this response:

```
HTTP/1.1 200 OK
x-amz-id-2: NZqHg8qXupykweLnUJgAweg4G9gQRjzyQv3qsxW/LdzEvg/1SGNAqJ0SafCQJArM
x-amz-request-id: C73715EFC96D2350
Date: Wed, 18 Nov 2015 21:37:23 GMT
Access-Control-Allow-Origin: https://kw.sg
Access-Control-Allow-Methods: GET
Access-Control-Max-Age: 3000
Access-Control-Allow-Credentials: true
Vary: Origin, Access-Control-Request-Headers, Access-Control-Request-Method
Last-Modified: Wed, 18 Nov 2015 19:58:34 GMT
ETag: "ddef5a2423e38521a0f5248d7ab3102e"
Accept-Ranges: bytes
Content-Type: application/xml
Content-Length: 4153
Server: AmazonS3
```

Note the above where I am able to get the Access-Control-Allow-Origin to return https://kw.sg as well.