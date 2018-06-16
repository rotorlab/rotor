---
layout: page
title: Server
permalink: /server/
---

Rotor server is a communication framework for work with remote objects from different devices. It makes the API development easier removing all getter/setter requests. Client data changes are replicated in all devices listening to the same object.

**Rotor philosophy** states that the only needed requests are those that change data on databases. That means that the rest of requests replaced.

<p align="center"><img width="80%" vspace="20" src=" {{ "/assets/shema_rotor.png" | absolute_url }}"></p>

Rotor libraries connected to Rotor and Redis servers. The first one controls object sharing queues, devices waiting for changes and all data edition on the remote database. The second gives us Pub/Sub messaging pattern for data changes replication.

When devices make changes in objects, client libraries send generated differences to Rotor server. This differences are applied in the database and replicated on the rest of devices which are listening to the same object.
