---
layout: post
title: Front-End Error Handling
prev:
  label: Cross-Domain Requests with CORS
  url: /articles/cross-domain-requests-with-cors
---

When building a web application, it's often a little too easy to focus only
on the golden path, building and testing only for situations when things go
right. Unfortunately, things rarely go right all the time in the real world.
Error handling is a vital part of any application's user experience, and if
done well, can leave your users feeling informed and properly considered.

Most errors that an application encounters can be grouped into a few
categories:

* **Input Errors:** Information provided by the user is unacceptable for
  some reason. This includes errors from form validation, duplicate actions,
  uniqueness issues, resources not found, etc.
* **Authorization Errors:** A user is attempting to perform an action to
  which he/she does not have permission.
* **Availability Errors:** A resource that is needed to complete the user's
  action is unavailable for some reason. This may be expected (scheduled
  maintenance) or unexpected (server crash!).
* **Unexpected Errors:** These are errors that likely indicate a bug in
  the application, such as unhandled exceptions.
  
Almost every application will have instances of each of these error categories
at some point. Handling each appropriately is key to keeping users who
encounter errors from becoming angry users.

### Front-End vs. Back-End Error Handling

In web application back-ends, expected errors are usually handled by displaying
or responding with some kind of error message, while unexpected errors will
short circuit the normal response process and display a generic error page.
Applications that are very poorly configured might even spit out internal
error details to the end user. For the most part, back-end applications are
not always good at helping a user recover from an error, but they are pretty
good at letting the user know something is wrong.

Front-end applications, for better or worse, have no built-in mechanism for
halting everything and displaying an error message. When a JavaScript
error occurs, usually one of three things happens:

1. The application keeps running, but something the user expected to happen
   doesn't happen. The most common user response to this type of error is
   simply to try the action again (and again) hoping it will "work this time."
2. The application stops running but displays no sign that it has stopped.
   The user will retry the action (or try to perform a different one) to no
   avail.
3. If the error happened early enough, the entire page may be prevented from
   being properly set up and the user will just see a white screen.

These scenarios, from a user experience perspective, are terrible. They are
likely to lead to user frustration, a feeling of helplessness, and eventual
anger. Front-end applications are in many ways more flexible than back-end
applications in how to handle and respond to errors, but **you must handle
the errors yourself.** A browser's built-in facilities will be little help
to the end user.

### Getting it Right

There are many ways to implement error handling in a JavaScript application.
You may define a global error handler that can display messages passed to it,
or have each component or control handle its own errors. 

One clean way to handle errors is to define a common schema for all of your
application's errors and use the browser's built-in event system to "bubble
up" errors that are then caught and handled appropriately. For instance,
a form validation error might be caught on the `form` element (or relevant
input) and displayed inline, while an unrecognized system error might bubble
all the way up to the `document` and display a generic message.

User communication when an error occurs is vital. You need to tell the user
what went wrong, but also what they should do now. In general those communications
will be:

* **Change Something, Try Again.** The user has entered some kind of invalid
  input, but with a few changes the action might succeed.
* **Try Again Later.** Something has gone wrong, but it's likely to work again
  soon. Check back in a while, and if it's still not working contact support.
  Depending on your application, this
* **Contact us.** Something is wrong in an unexpected place. Get in touch with
  support as this isn't likely to get better on its own.
  
When handling client-side errors, you often have another choice to make:
halt the application or allow it to continue running. If the error only applies
to part of your system, you may want to allow the user to continue using the
application. If the error is widespread or critical, however, you may want to
display the error message in an undismissable modal window (or simply replace
the page contents with the error message), preventing the user from fruitlessly
attempting further action.

### AJAX Errors and HTTP Status Codes

The simplest (and still a very effective) way to communicate general information
about an error is to properly utilize [HTTP status codes](http://httpstatus.es/).
These can tell you much about what has gone wrong with a request, and even without
additional information give you hints about what to do next. These codes are split
into 4XX codes (something is wrong with the request) and 5XX codes (something is
wrong with the server). The most common statuses for web application errors are:

* **400 Bad Request:** This is generally an input error, the most common instance
  being invalid user input (such as a malformed email address).
* **401 Unauthorized:** Use this error code when a user is either trying to access
  something without being logged in, or trying to access something they shouldn't
  (such as another user's data or admin functionality).
* **403 Forbidden:** The difference between this and 400 can be subtle, but a 403
  error generally means that the server understood the request (it's not an input
  error) but will not fulfill it. An example of this might be the entry of an
  expired coupon code.
* **404 Not Found:** The most well-known of all the error codes, this simply means
  that the requested resource could not be found (either because of a malformed
  URL or a deleted or moved resource).
* **409 Conflict:** While mostly meant to refer to versioning conflicts (two users
  trying to write to the same resource), this can also be used to indicate uniqueness
  constraints (e.g. "email has already been taken").
* **500 Internal Server Error:** This is the generic "something has gone wrong, but
  we don't know what error." It's the catch-all.
* **503 Unavailable:** The server is experiencing an outage, either planned or
  unplanned.
  
Know these codes, and know them well, and you'll be well on your way to handling any
error that comes your way via an AJAX request.

### Catching Errors

You can attach an error handler at a global level by using [window.onerror](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers.onerror).
Once attached, your handler can override the default browser behavior allowing you to
display information to the user as necessary.

```javascript
window.onerror = function(message, url, lineNumber) {
  // detect if the error is something you know how to handle
  if (errorCanBeHandled) {
    // display an error message to the user
    displayErrorMessage(message);
    // return true to short-circuit default error behavior
    return true;
  } else {
    // run the default error handling of the browser
    return false;
  }
}
```

While this will work, it's often difficult (if not impossible) to entirely parse out
the exact nature of a problem from a thrown exception. When handling AJAX errors,
for example, a better practice is to use your favorite library's AJAX error function
to detect the status code and react appropriately.

---

The most important takeaway for error handling is that **you need to do it.** Any
step towards informing the user when something goes wrong is a good one. Even an
`alert()` box is better than a silent failure. Remember, your application's user
experience encompasses the everything, not just the golden path.