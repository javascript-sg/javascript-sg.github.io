---
published: true
title: Learnings from CORS
layout: post
---
CORS is a little hard to understand. I'll give you a quick overview of what it is about.

There are two main modes of CORS:

1. basic CORS which is `withCredentials` set to `false`
2. preflight CORS which is `withCredentials` set to `true`

Let's look at the first case.

## Basic CORS

Basic CORS just want your server response to match an allowable origin. It isn't uncommon to see servers returning with

```
Access-Control-Allow-Origin: *
```

This means it matches any domain name.

If the server returns three of the same header, like this:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: *
```

...browsers interpret it as:

```
Access-Control-Allow-Origin: *, *, *
```

...which is invalid.

You will get the error message -- "XMLHttpRequest cannot load http://example.com. The 'Access-Control-Allow-Origin' header contains multiple values '*, *, *', but only one is allowed. Origin 'http://example.com' is therefore not allowed access."

## Preflight CORS

This has `withCredentials` set to true and it disallows `Access-Control-Allow-Origin` to be set to an `*`.

When the request sends a header with a specified `Origin`, it has to return the same value back as `Access-Control-Allow-Origin`.

For example, in the request you have header:

```
Origin: http://example.com
```

Expected server response is:

```
Access-Control-Allow-Origin: http://example.com
```

Returning an `*` is not allowed and you will be returned with an error.

Possible Apache2 (httpd) configuration could be:

```
SetEnvIf Origin "http(s)?://([\d\w\.]+)$" AccessControlAllowOrigin=$0
Header always set Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin
```

Possible nginx configuration could be:

```
add_header "Access-Control-Allow-Origin" $http_origin;
```