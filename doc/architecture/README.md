# Architecture

This part is dedicated to the architecture of Xen Orchestra. It will give you hints about how we build this software.

## Overview

Here is a diagram giving an overview of what Xen Orchestra is:
![](./assets/xo-arch.jpg)

Xen Orchestra is split in modules:
- the core is "[xo-server](https://github.com/vatesfr/xo-server)", a daemon dealing directly with XenServer or XAPI capable hosts. This is where users are stored, and it's the center point for talking to your whole Xen infrastructure.
- the Web interface is in "[xo-web](https://github.com/vatesfr/xo-web)": you are running it directly in your browser. The connection with "xo-server" is done via *WebSockets*.
- "[xo-cli](https://github.com/vatesfr/xo-cli)" is a new module allowing to send commands directly from the command line.

We will use this modular architecture to add further parts later. It's completely flexible, allowing us to adapt Xen Orchestra in every existing work-flow.

## XO-server

XO-Server is the core of Xen Orchestra. Its central role opens a lot of possibilities versus other solutions. Let's see why.

### Daemon mode

As a daemon, XO-server is always up. In this way, it can listen and record every event occurring on your whole Xen infrastructure. Connections are always open and it can cache informations before serve it to another client (CLI, Web or anything else).

### Central point

Contrary to XenCenter, each Xen Orchestra's client is connected to one XO-Server, and not all the Xen servers. With a traditional architecture:

![](./assets/without-xo.jpg)

You can see how we avoid a lost of resources and bandwidth waste with a central point:

![](./assets/with-xo.jpg)

### Events

Legacy interfaces use the "pull" model, requesting data every "x" seconds:

![](./assets/noevent.jpg)

It's **not scalable** and slow.

With XO < 3.4, we used events in this way:

![](./assets/semievent.jpg)

But interface was still lagging behind the server. With XO 3.4, we got a full event system, allowing instant display of what's happening on your infrastructure:

![](./assets/fullevent.jpg)

### Pluggable

It's really easy to plug other modules to XO-server, and extend or adapt the solution to your needs (see XO-web and XO-cli for real examples).

### NodeJS under the hood

[NodeJS](https://en.wikipedia.org/wiki/Nodejs) is a software platform for scalable server-side and networking applications. It's famous for its efficiency, scalability and its asynchronous capabilities. Exactly what we need! Thus, XO-server is written in JavaScript. 

## XO-web

This is probably the first thing you'll see of Xen Orchestra. It's the Web interface, allowing to interact with your virtual infrastructure. As a module for XO-web, it facilitates the everyday Xen administrator work, but also provide a solution to delegate some part of your infrastructure to other people.

### JavaScript

We are also using JavaScript for XO-web: we stay consistent from the back-end to the front-end with one main language. [AngularJS](https://en.wikipedia.org/wiki/Angularjs) and [Twitter Bootstrap](https://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29) are also powerful allies to our everyday development.

## XO-cli

After [a request from someone on our Github repositoryy](https://github.com/vatesfr/xo-server/issues/23), we decided to add the possibility to do some actions with CLI. Just few hours after the request, [we created XO-cli](https://github.com/vatesfr/xo-cli). It's a real example of the Xen Orchestra modularity, and how we can be flexible.

## Other modules

In our vision, we think Xen Orchestra can be more than "just an Web GUI for Xen". With modularity, we can imagine a lot of things: because XO-server is a daemon, it can record any activity and thus create performance reports. The next step is auto-migrate VM for adapting the need to the demand (reducing the costs to only what you need). Everything is possible!


**Congrats! You've reached the end of this doc. See the next part, [about how to install Xen Orchestra](../installation/README.md).**
