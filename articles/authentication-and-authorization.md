---
layout: post
title: Authentication and Authorization
prev:
  label: Routing URLs in Static Apps
  url: /articles/routing-urls-in-static-apps
next:
  label: Cross-Domain Requests with CORS
  url: /articles/cross-domain-requests-with-cors
---

Authentication is perhaps the single most common requirement of any application. Being able to
quickly and easily register for or log into a service can make a huge difference for the user
experience. In a traditional web application, this is usually done using server-side session
tracking in one form or another. Static applications can keep track of sessions, too, but running
in an untrusted client-side environment means that extra care must be taken to ensure that a
user is verified and trustworthy.

Before we continue it's important to distinguish between **authentication** and **authorization**.
Authentication is the process of verifying that a user is who she says she is; this is your typical
log in system. Authorization is verifying that users can only perform as many actions as you want
them to perform and no more. Making sure that users can't read private data of others, perform
administrative actions, etc. fall into the purview of authorization.

### What is an Untrusted Environment?

One of the big differences between client-side and server-side web application code is that
**the end user can execute arbitrary code on the client-side without prior permission**. This
means that anything your application *can* do using JavaScript, a user can easily do just by
opening the Developer Tools in her browser. This carries two major consequences:

1. Applications must place authorization outside the purview of the client-side code, since it
   could be arbitrarily modified by any party.
2. Client-side application code cannot contain any "secret" information such as API keys or
   any kind of access credentials that aren't specific to the current user.

While untrusted environments pose a challenge and bear potential for security vulnerabilities,
building for such an environment forces you to focus on secure architecture early. Static apps don't have the
luxury of security through obscurity or other half-measures. So what are you to do?

A good rule of thumb is to **give your users enough leeway to ruin their own data, but not anyone
else's**. Remember, you're protecting your application against malicious users, not trying to tie
your development time up with unreasonable security constraints. If a user tries to hack your application and
in the process destroys their own user account data, that's not a problem. It's only when actions
might affect, compromise, or destroy the data of other users that you need to worry.

### Temporary Revocable Access Credentials

Unless you are building an offline-only application designed to store data only in the local browser,
your authentication process is going to involve a server. The simplest means of authentication works
generally like this:

1. The client directs the user to a server-side authentication process. This may be a JavaScript
   pop-up window or a browser redirect triggered by setting `window.location`.
2. The server authenticates the identity of the user via password, social sign-in, or
   other means.
3. The server creates a randomly generated token and associates it with the now authenticated user.
4. The server transmits the token back to the client.
5. The client includes the provided token on subsequent requests to the server as a proof of identity,
   granting the user access to protected resources.
   
Because the token is generated at the time of login and is random and unguessable, its presence serves
as proof enough that the request comes from the user to whom the token was assigned. A token that grants
access without any additional requirements is known as a **bearer token**.

Before token-based authentication became prevalent, many application APIs simply used bare username and password credentials
passed along in the request. Token-based authentication is superior to such a system for a number of reasons:

1. Credentials, especially user-provided passwords, should be stored as securely and infrequently as possible.
   Especially if an application offers third-party access to its server-side resources, direct password usage
   is a recipe for a security disaster that may compromise a users' accounts outside your own application.
2. The server can revoke access to a generated token at any time. If a user has reason to believe that one of
   their authorized tokens has been compromised in some way, it is simple to revoke access to the token without
   universally resetting a user's login credentials or all granted tokens.
3. In addition to manual revocation, tokens can be automatically expired or require additional proofs that they
   should remain valid. In essence, token-based authentication gives you the power to fully control your application's
   authentication and authorization flow.
   
Note that because bearer tokens grant immediate access to anyone possessing the token, **it is vital that any communication
that includes a token take place over an SSL-encrypted connection.**

### Digging Deeper

Authentication and authorization are rich fields, and you will need to give serious thought to whatever strategies you
decide to employ. While bearer tokens seem like a simple solution, they are vulnerable to [replay attacks](http://en.wikipedia.org/wiki/Replay_attack)
and if intercepted can completely compromise a user's account. Here are some tips to consider (and topics you can investigate
further if interested) for creating better user security for your static app:

* Don't re-invent the wheel. Lots of people have spent lots of time thinking about these topics. You're best sticking
  with an existing protocol like [OAuth 2.0](http://tools.ietf.org/html/rfc6749) rather than trying to come up with your own.
* Don't allow your access tokens to last forever. Use [refresh tokens](http://tools.ietf.org/html/rfc6749#section-1.5) or
  otherwise require period reauthentication to ensure that a client's stale credentials don't become the basis for a
  security breach.
* Don't store access tokens in plaintext, or in `localStorage`. Even though users will be able to undo any kind of client-side
  encryption of tokens that you do, it is a good practice to obfuscate the token before storing it on the client and also
  to store it somewhere that expires such as a cookie or `sessionStorage`, rather than the long-lived `localStorage`.
* Use authorization scopes. When a client authenticates, make it explicitly request the various resources to which it wants
  access, even if it is completely under your control. If a client only has access to what it needs, it can't mess with
  anything it doesn't.

### Third-Party Authentication

Depending on the needs of your application, you may not need to implement your own custom token-based authentication.
If you are building an application that primarily interacts with another service, there may be dedicated JavaScript
libraries available (e.g. [Facebook's JS SDK](https://developers.facebook.com/docs/facebook-login/login-flow-for-web/))
or they may offer ways to create authorization tokens manually (e.g. [GitHub's API](http://developer.github.com/v3/oauth/#create-a-new-authorization)).

A relatively new option that is growing in popularity is a dedicated back-end authentication provider. Services such
as [Firebase](http://firebase.com) and [UserApp](http://userapp.io) offer simple ways to authenticate users to your
static application without running any server-side code yourself. Each service will have its own strengths and weaknesses
and should be considered as part of your overall authentication strategy.

One final (and extremely promising) option for authentication is [Mozilla Persona](http://www.mozilla.org/en-US/persona/).
While it currently offers simple email-based authentication in a cross-browser way, the long-term goal of the project is
to bring authentication directly into the browser itself. If this goal is realized, it may become possible to have a
trusted authentication scheme that doesn't rely on any server whatsoever.

---

Phew, that was a lot of information! Properly handling user authentication and authorization can be complex, and the specific
needs of your application will dictate how it takes shape. However, once you've conquered the challenge, you are much, much
closer to a functional static web application.