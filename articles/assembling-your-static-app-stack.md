---
layout: post
title: Assembling Your Static App Stack
prev:
  label: Thinking With Services
  url: /articles/thinking-with-services
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

### Build Tools

Static web applications are stored on the server and sent to the browser as HTML,
CSS, and JavaScript files, but they don't necessarily begin that way. Build tools help developers
perform important tasks such as combining multiple files into one, compiling preprocessor languages,
or even packaging code into a mobile application.

Different tools will be created with different purposes. Some are built for a specific purpose
while others can be used for just about any application you might build. When choosing a build
tool, it makes sense to find something that fits your workflow, is actively maintained, and
has an active user community.

**Examples:** [Jekyll](http://jekyllrb.com/), [Grunt](http://gruntjs.com/), [Yeoman](http://yeoman.io/)

### Package Managers

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

### Back-End Services