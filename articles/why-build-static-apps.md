---
layout: post
title: Why Build Static Web Apps?
next:
  label: Thinking in Services
  url: /articles/thinking-in-services
---

Rather than assemble content in server-side processes, static web applications rely on the user's browser to drive interaction and content rendering. Once considered only useful for static content, modern static web apps can dynamically fetch data, synchronize multiple users in real time, and more. But why would you *want* to build a static web application?

**Broad Appeal.** Static web applications are built using HTML, JavaScript, and CSS. While web developers may specialize in languages and frameworks such as Ruby, Python, or .NET, just about every web developer (as well as many designers, entrepreneurs, and students) is familiar with these universal building blocks of the web platform.

**Rapid Development.** Using modern frameworks such as [Bootstrap](http://getbootstrap.com) and [AngularJS](http://angularjs.org), and tools such as [Divshot](http://www.divshot.com/), you can quickly "sketch out" an interface and turn it into a rough static prototype. That prototype can then become your application by hooking it to back-end data services.

**Simple Scalability.** Static web apps are just files stored on a web server and sent to the end user as-is. They can be scaled to millions of users with off-the-shelf technology. You'll still need efficient, scalable back-end services, but such services are increasingly available from third party vendors. Static apps let you focus on your application, not your infrastructure.

**Day One Modularity.** All web applications at scale separate into some form of modular architecture. This allows developers to build pieces of the overall application using specialized technologies that are well-suited to specific problems. Static web applications encourage this modularity from the beginning by forcing all back-end data to be provided by services separated from the front-end interface.

**Increased Flexibility.** It's easy to build alternative interfaces when front and back ends are separated. Exposing a public data API, building a command-line client, creating administrative control panels, and more are much simpler to implement when the back-end and the front-end are built independently.

Of course, static web applications come with their own challenges. Code in the browser must be carefully designed to avoid security holes and exploitation. It can be difficult to make dynamic content in a static web application available to a search engine. The user experience must be carefully optimized to avoid a perceived lag as the application interface and data are loaded independently. Each of these challenges can be overcome, and each will be explored in a future chapter of this guide.