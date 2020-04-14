---
layout: page
group: reference
title: Network architecture
---

![Network architecture](/assets/network_architecture.png)

## High-level overview

- Data from an **upstream timing provider** is fetched or received by a **timing
  service**, converted into CTD format, and published to the master router. The
  data is also sent to a per-service analysis module.
- The analysis module updates its internal state and publishes CTD-format
  analysis data to the master router.
- The master router (a Crossbar WAMP router) publishes all data to **timing
  relays** subscribed to the network.
- The **DVR process** runs within the master router and logs all received
  timing data to disk in order to generate replay files.
- The timing relays republish messages to **timing clients** (e.g. users of
  the Timing71 website).

## Timing service protocol detail

The Timing71 network is built over [WAMP](https://wamp-proto.org/) and as such
uses primitives of _publish_, _subscribe_, _register_ and _call_.

- When a timing service first starts, it authenticates with the master router,
  connects to the `timing` realm, and publishes its
  [service manifest]({% link reference/manifest.md %}) to the `CONTROL` topic.
  It also registers several RPC endpoints, including a "liveness" check.
- A **directory process** records all current service manifests, and
  periodically checks for liveness of services, removing dead services from its
  list. It periodically publishes a
  [directory listing]({% link reference/directory_listing.md %}) message to the
  `DIRECTORY` topic. This publish has the `retain` flag set so that new
  subscribers automatically receive the most recent message for the topic.
- Periodically, and usually at approximately the interval declared in its
  manifest's `pollInterval`, a timing service will publish a
  [service state message]({% link reference/state.md %}) to a topic derived from
  [RPC]({% link reference/constants.md %}#rpc).STATE_PUBLISH and the service's
  UUID.
- At the same time, the most recent service state is passed to the associated
  analysis process. Periodically, the analysis process will publish any changed
  state to a topic derived from
  [RPC]({% link reference/constants.md %}#rpc).ANALYSIS_PUBLISH, the service
  UUID, and the key associated with the analysis data. See
  [analysis update]({% link reference/analysis_update.md %}) for details.
- The **DVR process** is automatically subscribed to all state and analysis
  published messages, and records them per UUID. If no data is received for a
  service for ten minutes, the service is assumed to have been shut down, and a
  recording file is compiled.

## Timing client protocol detail

Clients may connect to any of the available timing relays. They need not
authenticate.

- Clients first obtain a list of current relays via
  [https://www.timing71.org/relays](https://www.timing71.org/relays) - a list of
  URLs and current number of connections is given, and clients are encouraged to
  connect to the relay with the fewest current connections.
- Clients then connect to the timing relay via WAMP.
- Clients should subscribe to the `DIRECTORY` topic to receive a list of current
  timing services.
- Clients may then subscribe to the desired service topic (remember, derived
  from [RPC]({% link reference/constants.md %}#rpc).STATE_PUBLISH and the
  service's UUID as given in the manifest listing).
- Clients may also choose to subscribe to analysis update messages. There are a
  range of topics required for a complete analysis state. `topicPrefix` is
  formed as `livetiming.analysis/<service UUID>`.
  - `${topicPrefix}/car_messages` (prefix-matched)
  - `${topicPrefix}/driver`
  - `${topicPrefix}/lap` (prefix-matched)
  - `${topicPrefix}/messages`
  - `${topicPrefix}/session`
  - `${topicPrefix}/static`
  - `${topicPrefix}/stint` (prefix-matched)

  See [analysis update]({% link reference/analysis_update.md %}) for details.
- There are a few RPC call endpoints available:
  - To request a full copy of the current analysis state, clients should call
    [RPC]({% link reference/constants.md %}#rpc).REQUEST_ANALYSIS_DATA
    (substituting the service's UUID).
