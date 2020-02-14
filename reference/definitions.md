---
layout: page
group: reference
title: Definitions and terms
---

- **Client** - consumer of live timing data.
- **CTD format** - Common Timing Data format, as detailed on this site.
- **Master router** - the central node of the timing network. Responsible for
  passing data from service processes to relay nodes and for orchestration of
  other WAMP components.
- **Relay** - a process that subscribes to timing data updates, and passes them
  on to clients subscribed to the relay.
- **Service** - a process that obtains timing data from an upstream source,
  converts it to the CTD format, and publishes it over the timing network.
- **Service manifest** - a description of the contents of state messages from
  a service process. See [Service manifest]({% link reference/manifest.md %})
- **Upstream source** - live timing data provider, typically based at the track;
  think Al Kamel, TimeService, etc.
- **WAMP** - [Web Application Messaging Protocol](https://wamp-proto.org/), an
  open-standard WebSocket subprotocol with publish/subscribe and remote
  procedure calling.
