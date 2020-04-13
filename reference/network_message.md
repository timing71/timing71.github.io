---
layout: reference
group: reference
title: Network message wrapper
---
```json
{
  "msgClass": 4,
  "date": "2020-04-13T20:23:00+01:00",
  "retain": true,
  "payload": {...}
}
```

Messages sent via the WAMP router are given this wrapper.

## Description

- **msgClass**: _Required_. Identifies the type of message contained in
  `payload`. One of the constant values defined in
  [the `MessageClass` enum]({% link reference/constants.md %}#messageclass).
- **date**: _Required_. Date of transmission of the message.
- **retain**: Boolean. Routing information working around
  [a bug in Crossbar](https://github.com/crossbario/crossbar/issues/1242);
  when `true`, tells relay servers to rebroadcast this message with the `retain`
  WAMP publish option.
- **payload**: _Required_. The message content, as defined by the appropriate
  message class definition.
