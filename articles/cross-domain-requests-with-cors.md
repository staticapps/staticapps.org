---
layout: post
title: Cross-Domain Requests with CORS
prev:
  label: Authentication and Authorization
  url: /articles/authentication-and-authorization
next:
  label: Front-End Error Handling
  url: /articles/front-end-error-handling
---

**Cross-Origin Resource Sharing (CORS)** is a powerful technology for static web apps. To understand
what it is and why it's important, you first need to understand a bit about how browsers work.

### The Same-Origin Policy

The [Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Same_origin_policy_for_JavaScript)
restricts the browser from performing certain actions by scripts or documents based on
the **origin**. The origin is everything in the URL before the **path** (for example,
`http://www.example.com`). For certain actions, the browser will compare origins and, if
they don't match, won't allow things to proceed. For example:

* A parent document can't access the contents of an `<iframe>` that comes from a different
  origin. This prevents a malicious site from opening up your bank's website and stealing
  your credentials, as an example.
* While one document can send information to another via a form post, AJAX
  requests across origins are generally disallowed.
  
The Same-Origin Policy is a vital piece of web security architecture, but it also poses
a problem. What happens when you **want** to allow a site with a different origin to
access your content? For example, what if you're providing a JSON API for third-party access,
or even just for your own use?

### Enter Cross-Origin Resource Sharing

CORS allows you to more cleanly separate your front-end from your back-end. This modular
approach makes it easier to scale your application as you reach more users. It is also
powerful when companies (such as GitHub or Amazon Web Services) provide CORS-enabled APIs
as they allow you to build novel interfaces or mash-ups without needing to build your own
back-end.

If you are building an application using a third-party data provider or an API that already
supports CORS, there isn't much else you need to know! You just make normal AJAX requests
from the browser and everything will "just work." If, on the other hand, you are building a 
custom back-end to provide data to your static app, you will need to know a bit more.

### Implementing CORS

CORS is a group of special response headers sent from the server that tell a browser whether
or not to allow the request to go through. There are two types of CORS requests:

1. Simple `GET` and `POST` requests that act very similarly to submitting a form on a website.
2. More complex requests using other HTTP methods (such as `PUT`), add Authorization headers, etc.

For a simple request to be allowed cross-domain, the server simply needs to add the
`Access-Control-Allow-Origin` header to the response. This header needs to either be equal to
the origin of the request or `*` to indicate that any origin is allowed. So if the server
responded with:

    Access-Control-Allow-Origin: http://example.com
    
Then only requests from `http://example.com` will be allowed. Note that only one origin can be
specified in this header; if a server wants to restrict access to multiple domains it will need
to dynamically return the origin provided by the request.

### Preflighted CORS Requests

For more complex requests, the browser will "preflight" the request by sending an `OPTIONS` request
to the server first. This request is basically there to ask the server if the full request is
permissable. Note that while this requires extra setup on the server side, the browser will do this
automatically depending on the characteristics of the request.

The server will respond to this `OPTIONS` request with a variety of headers that specify what is and
isn't allowed:

* `Access-Control-Allow-Origin`: As described above, this needs to be either the origin of the request
  or `*`.
* `Access-Control-Allow-Methods`: This is a comma-separated list of the HTTP methods that are allowed,
  for example `POST, PUT, OPTIONS`.
* `Access-Control-Allow-Headers`: A comma-separated list of allowable custom request headers, for example
  `AUTHORIZATION, X-CLIENT-ID, X-CLIENT_SECRET`.
* `Access-Control-Max-Age`: The amount of time (in seconds) that the browser is allowed to cache the
  response to this preflight request.
  
If this seems daunting or confusing, don't worry. Most web development stacks will have libraries available
to help you manage CORS headers appropriately. Which headers you will need depends on your specific application.

### A Common Example

Let's take one of the most common use cases for CORS: accessing application data from a JSON API on another
server. Let's say we want to `GET` data from `http://api.example.com/user` while visiting `http://www.example.com`.
We have an access token, (e.g. `"ABC123"`), that needs to be passed to the API server in the `Authorization`
header to authenticate the request. When we create the AJAX request with the custom header, the browser
will send an `OPTIONS` request to the API server with some relevant headers:

    OPTIONS /user
    Origin: http://www.example.com
    Access-Control-Request-Method: GET
    Access-Control-Request-Headers: Authorization
    
This lets your API server know the method and headers of the request that's about to happen. Since this is a
request that you would want to allow, you would then respond to the request with these headers:

    Access-Control-Allow-Origin: http://www.example.com
    Access-Control-Allow-Headers: AUTHORIZATION
    Access-Control-Allow-Methods: GET
    
Now the browser will proceed to send the actual `GET` request to your API server, and you can respond to that
as you would normally.

---

Hopefully this article gave you an idea of how you might implement CORS in your back-end or use it in your
static web application. Cross-domain requests were once frustrating and unreliable, requiring bizarre hacks
and workarounds. Thanks to CORS, it's now simple, straightforward, and fully supported in all major browsers!
