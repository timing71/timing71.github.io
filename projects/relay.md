---
layout: page
title: Timing71 relay
---
The Timing71 architecture relies on several data relays for load-balancing.
These relays connect to the master server and disseminate messages to clients.
Clients connect to the relays, not the master server.

The relay code itself is a
[WAMP](https://wamp-proto.org/) component, designed to run as part of a WAMP
router such as [Crossbar](https://crossbar.io/).

## Running through Docker

The easiest way to run a relay is through a Docker container:

```bash
docker run \
       -e MASTER_URL=<master relay URL> \
       -e RELAY_URL=<URL at which this relay can be reached> \
       -e LIVETIMING_RELAY_KEY=<relay key> \
       -p 8080:80 \
       registry.gitlab.com/timing_71/relay:latest
```

which will run a relay and expose it on port 8080 on the host machine.

In order to be added to the list of available relays used by the website, a
relay must have a valid `LIVETIMING_RELAY_KEY` set. Without the key, a relay
can still connect to the master server, but will not be used by clients (unless
they explicitly specify the URL of the relay to connect to).

## Running directly in Crossbar

The relay repository contains an example Crossbar configuration file.
Configuring Crossbar is beyond the scope of this document.

## HTTPS

You probably want to proxy access to the relay and include an HTTPS layer.
[nginx](https://www.nginx.com/) can work fine for this, or consider an
application router such as [Traefik](https://docs.traefik.io/). Make sure you
configure your relay's `RELAY_URL` environment variable with the _externally
accessible_ URL, including port number.

Specific configuration is beyond the scope of this document.
