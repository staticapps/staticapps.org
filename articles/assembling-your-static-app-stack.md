---
layout: post
title: Assembling Your Static App Stack
prev:
  label: Thinking With Services
  url: /articles/thinking-with-services
next:
  label: Routing URLs in Static Apps
  url: /articles/routing-urls-in-static-apps
---

Once you've decided to build a static web application, there are still several important choices
to be made in terms of your application's technology stack. While a simple static web application
can be written entirely inside a single HTML file (with embedded CSS and JavaScript), complex
applications benefit greatly from the wide variety of tools and services available for static
web development. Static web applications generally utilize some combination of the following
categories of tools:

### Preprocessor Languages

Preprocessor languages are programming languages that are compiled into HTML, JavaScript, or CSS.
They may offer a terser syntax, additional features, or other benefits over the base language.
While some of these languages are written in JavaScript and can be parsed and run entirely in the
browser, these languages are generally compiled into their target language before being deployed
to a web host.

**Examples:** [CoffeeScript](http://coffeescript.org), [LESS](http://www.lesscss.org/), [HAML](http://haml.info/), [Dart](https://www.dartlang.org/)

### Build Tools / Site Generators

Static web applications are stored on the server and sent to the browser as HTML,
CSS, and JavaScript files, but they don't necessarily begin that way. Build tools help developers
perform important tasks such as combining multiple files into one, compiling preprocessor languages,
or even packaging code into a mobile application.

Different tools will be created with different purposes. Some are built for a specific purpose
while others can be used for just about any application you might build. When choosing a build
tool, it makes sense to find something that fits your workflow, is actively maintained, and
has an active user community.

**Examples:** [Jekyll](http://jekyllrb.com/), [Grunt](http://gruntjs.com/), [Yeoman](http://yeoman.io/), [Harp](http://harpjs.com/)

### Package Managers

Package managers allow developers to easily install open source or shared library code that is meant
to work across multiple applications. Using a package manager, you can declare **dependencies** for
your project that the package manager will automatically download and install into a usable location.
This makes it easier both to add new libraries and update existing libraries in your projects.

**Examples:** [Bower](http://bower.io/), [Component](http://component.io)

### JavaScript Frameworks

Your JavaScript framework provides you with tools and methodologies to organize your application's logic
and data structure as well as (often) ways to render templates and/or manipulate HTML on the page. Your choice
of framework is one of the most defining aspects of your application as it has a huge impact on everything
from project file structure to how your markup is written.

Different frameworks utilize different methodologies such as Model View Controller (MVC) or Model View
ViewModel (MVVM). Each has its own strengths, weaknesses, and communities, and it is well worth your time to
investigate options thoroughly before settling on one.

**Examples:** [Angular](http://angularjs.org/), [Ember](http://emberjs.com/), [Backbone](http://backbonejs.org/)

### UI Frameworks

A UI framework can help you quickly implement common interface patterns by providing sensible default styles and
behaviors for your application. Useful both for rapid application prototyping and as a basis for production code,
these frameworks give you elements such as buttons, grid layouts, well-styled forms, and more.

**Examples:** [Bootstrap](http://getbootstrap.com), [Foundation](http://foundation.zurb.com)

### Static Web Hosts

Of course, you'll need a place to host your project once you're ready to bring it online. There are many options
for static web hosting including traditional shared hosts, online storage with hosting, and new dedicated static
web hosting services. When choosing a host, there are a few things (other than cost) to look for:

* Does the host provide any kind of content acceleration through a global server network?
* Can I host an SSL-protected static web app on the provider?
* How easy is the deploy process? Are there developer-friendly features like rollback?

**Examples:** [Divshot](http://www.divshot.io/), [Amazon S3](http://aws.amazon.com/s3/), [GitHub Pages](http://pages.github.com/)

### Back-End Services

Back-end service providers offer a way to implement a part of your application's business needs without having to host
or build custom code for it. These services may provide database storage, user authentication, email delivery, or
a variety of other services. Some back-end services try to offer a comprehensive package while others focus on a narrower
slice of your application needs.

**Examples:** [Firebase](http://www.firebase.com/), [UserApp](http://userapp.io), [Hull](http://hull.io)

### Third-Party APIs

If you're building a static web app and would like to integrate with data from a third-party API, you're in luck!
Many API providers now offer either dedicated JavaScript libraries for direct browser integration or support static
web apps through [Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/HTTP/Access_control_CORS).
Connecting to APIs in-browser saves you the duplicated infrastructure cost of proxying requests through your own servers.

**Examples:** [AWS SDK for Browser](http://aws.amazon.com/sdkforbrowser/), [GitHub API](http://developer.github.com/v3/)

### Custom Server APIs

As you build out your application, you may find there are pieces that can't be handled by an off-the-shelf third-party
provider. That's ok, and totally in keeping with a static application mindset! When you run into these cases, it's best
to build the functionality you need as a compact, independent, purpose-driven service that can be easily consumed by the
browser and any other platforms you support. 

---

Choosing a technology stack that works well for you will be vital to the productivity and maintainability of your project.
What works well for one application may not work well for another, but one of the benefits of building static is that
you have more flexibility to compose an ideal, modular stack that suits your needs.
