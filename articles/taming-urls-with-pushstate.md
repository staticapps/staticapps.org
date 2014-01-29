---
layout: post
title: Taming URLs with pushState
prev:
  label: Assembling Your Static App Stack
  url: /articles/assembling-your-static-app-stack
---

User-friendly, readable URLs are a key feature of any web application. More than just the way a user
gets to your application, URLs are a vital part of the user experience when it comes to navigation,
sharing, and search engine friendliness:

* They provide immediate context to users about the structure and content of your site.
* URLs that accurately reflect application state can be navigated using the browser back/forward buttons.
* If your application has publicly shareable content, a descriptive URL will help engage faster.
* Many search engines use URL structure to analyze for content keywords.

If you're building a dynamic JavaScript-driven application there are effectively two options for URL
navigation/link support:

1. Abuse the hash location (the part of the URL that comes after `#`).
2. Utilize JavaScript's `pushState` mechanisms to alter the URL without forcing content to reload.

While browser support and other considerations once made hash navigation a necessary evil, today there is no
reason to utilize URL fragments over properly state-controlled URLs.

### Managing the History Stack

HTML5 has brought the concept of programmatic manipulation of history. This is exposed to developers in
the form of a stack available at `window.history`. There are three four ways you will interact with the stack:

1. `history.pushState(stateObject, title, url)` adds a new state to the history stack. This is what you'll
   want to do when the user has performed an action that you want to record as a navigation event.
2. The `history.replaceState` works just like `pushState` but changes the current
   history entry instead of adding a new one.
3. When a navigation event occurs (for example, the user hits the **Back** button or methods like `history.go(-2)`
   are called), a `popstate` is triggered on the `window`.
4. You can read the current history state at `history.state`
   
Using these simple methods you can create complex in-page navigation that works perfectly with the browser's built-in
navigation buttons.
   
### A Simple Example

When utilizing `pushState` you have two sources of information: the current path of the application and any information
that has been put on the history stack as a state object. Let's imagine a simple Twitter-like application and how we might
manage history. If a user started on the home page and clicked on a username, we might have handling code like this (using
jQuery-based code as a quick common denominator):

```javascript
// For any link that starts with "/users"
$('a[href^="/users/"]').on('click', function() {
  // Push the new URL onto the history stack
  window.history.pushState({}, "", $(this).attr("href"));
  
  // ... load the user info and display it using JavaScript
  
  return false; // don't let the browser navigate
});
```

Notice that in this example we aren't adding anything to the state object. That's because in this case, the URL alone (for example,
`/users/alice` is all the information we need. However, now let's imagine that the user clicks on a "Compose Message"
that opens a modal dialog box. The URL of the page doesn't need to change, but the state has changed.

```javascript
$('#compose-message').on('click', function() {
  // by leaving out the last argument, we change the state object but keep the same URL
  window.history.pushState({compose: true}, "");
  
  // ... open the compose modal
  
  return false;
});
```

So what has pushState gained us in this example? Well, nothing, unless we also create a handler method for when the user navigates
through the history stack:

```javascript
$(window).on('popstate', function(e) {
  // the state object is attached to the native event
  var state = e.originalEvent.state;
  var path = window.location.pathname;
  
  if (state && state.compose) {
    // ... open the compose modal
  }
  
  if (path.indexOf('/users') == 0) {
    // ... load user data  
  }
});
```

This is a simplified example that doesn't deal with many edge cases, but what we've essentially done is build a simple
routing and navigation state system that allows us to fully leverage the browser's history. For more detail on the specific
method calls, see [Manipulating the Browser History](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Manipulating_the_browser_history)
on the Mozilla Developer Network.

### A Word of Caution

When using `pushState` you must **make sure that your server will deliver proper content, no matter what URL the user visits**. In our
example above, if we create client-side code to handle a `/users/*` path but the server doesn't know to serve up the same
`index.html` file for all `/users/*` paths, you may end up with broken links.

Make sure if you're using `pushState` that you are properly configuring your static web server to serve the correct content for all relevant
URLs, or use a host with pushState support built-in such as [Divshot](http://www.divshot.io).

### Routing Libraries

In the real world, you almost certainly won't "roll your own" pushState handler system; you'll use a library. Most front-end frameworks
include their own routing system (e.g. [Backbone Router](http://backbonejs.org/#Router) or [ngRoute](http://docs.angularjs.org/api/ngRoute)),
but knowing the underlying mechanics will help remove the "magic" fear of HTML5 routing and navigation.

If you're looking for a stand-alone routing library, [Page.js](http://visionmedia.github.io/page.js/) lets you take full advantage of
`pushState` and state objects with a simple, declarative API.