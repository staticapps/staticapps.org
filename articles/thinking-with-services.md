---
layout: post
title: Thinking With Services
prev:
  label: Why Build Static Apps?
  url: /articles/why-build-static-apps
---

Static web applications have very clearly defined constraints, and as such lend
themselves naturally to **Service-Oriented Architecture (SOA)**. An application
built with SOA will be made up of many independent pieces (services) coordinating
together to achieve the overall goals of the application.

Building an application with service orientation comes with three key benefits:

1. Each service is built independently, and so they can utilize technologies that
   are particularly suited to their individual problem spaces. A search service
   might look very different from an email delivery service, for instance.
2. Clear boundaries between services often mean more scalable user experiences.
   Services can be optimized individually, and one service experiencing an outage
   doesn't necessarily take down the entire application.
3. Services can often be structured in such a way that they can be implemented wholly
   by third parties.

Service-orientation isn't just a theoretical model for successful software. The biggest,
most traffic-heavy applications on the web today use SOA to build faster and more resilient
applications.

> Each service could be revved on its own deployment schedule, often weekly, empowering
> each team to deliver innovation at its own desired pace.  We unwound the centralized 
> deployment team and distributed the function into the teams that owned each service. 
> <span class="attribution"><a href="http://techblog.netflix.com/2012/06/netflix-operations-part-i-going.html">Netflix</a> on transitioning to SOA</span>

Amazon famously [received a mandate from Jeff Bezos](http://apievangelist.com/2012/01/12/the-secret-to-amazons-success-internal-apis/) (all the way back in 2002!) that all
teams would expose their work through internal API services that must be built in a way
that they could be externalized (made available to the public) at any time. This mandate
is believed to be at least partially credited with the creation of Amazon Web Services.

---

So how do static web apps fit into the SOA picture? In many ways, you can think of a
static web app as a **User Interface Service**. It has the responsibility of
connecting to and coordinating with your application's other services to provide
a cohesive, intuitive experience for a particular end user.

Interestingly, this means that it can be very simple to build multiple static app UIs
for the same application. You might build one interface application for end users and
and entirely separate one for application administrators, as an example. Service-Oriented 
Architectures can be implemented in a wide variety of ways, though for our purposes the 
communication channels between services are likely involve sending and receiving 
JSON data over HTTP.

As you think about the best way to structure your application, keeping in mind the
modularity and benefits of Service-Oriented Architecture can help you make the right
choices the first time. As you'll see in future chapters, the browser can make a 
great coordinating agent for a myriad of web services.