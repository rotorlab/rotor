---
layout: page
title: Database
permalink: /database/implementation/
---

Database is a complementary module for Rotor Core. It allows to work with shared (Java) objects between many devices offering users real time changes and better mobile data consumption. 

Forget things like swipe-to-refresh events, lots of server requests and object storage management. 

**Rotor Database philosophy** states that the only needed requests are those that change data on remote database. That means that the rest of requests you are imaging are removed now.
 
<p align="center"><img width="80%" vspace="20" src=" {{ "/assets/shema_rotor.png" | absolute_url }}"></p>
 
Rotor Core is connected to Rotor and Redis servers. The first one controls object sharing queues, devices waiting for changes and all data edition on remote database. The second (as you probably know) gives us Pub/Sub messaging pattern for data changes replication.