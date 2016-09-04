---
published: true
title: Thoughts on AWS Lambda
layout: post
tags: [amazon, aws, s3, lambda, api, microservice]
---
Amazon Web Services has a product called Lambda. It made me rethink what micro-services is about.

The idea isn't a new one, you send your source code up to AWS and it prepares it onto a server and exposes an `index.handler`. What this handler does is that it exposes a set of API that you can call using REST. 

The only downside is that you have to upload a ZIP source code all the time to deploy this. Or you could do this through pushing it to S3. In either cases you have this rudimentary interface to open up some really progressive ideas on micro-services.

I may be overthinking here. The terrible interface to update my files worked in its advantage, it encourages me to not deploy too many times. What I find myself doing is splitting up my code into small pieces, after all, each time some piece of code reaches a level of stability, it doesn't have to be touched anymore. And that in turn reinforces the idea of micro-services -- doing just one small bit super reliably.

Great job Amazon! I'm excited over what more it can do.