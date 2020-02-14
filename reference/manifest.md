---
layout: reference
group: reference
title: Service manifest
---
A service manifest contains metadata relating to the content of state messages
from a service process. It defines the meanings of each column of data for
each car, and contains information about the data source. State messages cannot
be correctly understood without a service manifest.

The manifest for a running service instance may change over time; for example,
between two track sessions happening back-to-back.

{% contentfor example %}

```json
{
  "colSpec": [
    ["Num", "text", "Car number"],
    ["State", "text"],
    ["Class", "class"],
    ["PIC", "numeric", "Position in class"],
    ["Driver", "text"],
    ["Gap", "delta", "Gap to leader"],
    ["Int", "delta", "Interval to car in front"],
    ["S1", "time", "Sector 1 time"],
    ["S2", "time", "Sector 2 time"],
    ["S3", "time", "Sector 3 time"],
    ["Last", "time", "Last lap time"],
    ["Best", "time", "Best lap time"]
  ],
  "description": "4H of Sepang - Free practice 1",
  "livetimingVersion": {
    "core": "2020.1.1",
    "plugin": "2020.1.4"
  },
  "name": "Asian Le Mans Series",
  "pollInterval": 1,
  "serviceClass": "aslms",
  "source": ["&copy; TimeService", "http://www.timeservice.nl/"],
  "trackDataSpec": [
    "Air temperature",
    "Track temperature"
  ],
  "uuid": "99e54c1c603b4e318c4c8afac88f94e7"
}
```

{% endcontentfor %}

## Description

- **colSpec**: _Required_. Defines each of the columns of data available for
  display, as used in the `cars` array in state messages.
  Each item consists of a two- or three-element array:
  - Column heading
  - Type
  - Description _(optional)_
- **description**: _Required_. Description of the session or race.
- **livetimingVersion**: Version information relating to the service instance.
  - **core**: Version of `livetiming-core` in use.
  - **plugin**: Version of the specific timing service plugin.
- **name**: _Required_. Name of the racing series or upstream provider.
- **pollInterval**: _Required_. Interval at which new data is obtained from the
  upstream data source. Used to indicate accuracy of calculated data.
- **serviceClass**: Name of service plugin.
- **source**: Attribution of source data. Consists of a two-element array:
  - Name
  - URL (or `null`)
- **trackDataSpec**: _Required_. Defines the data available in the state
  `session.trackData` element.
- **uuid**: Unique identifier generated for this service instance.

The service manifest _may_ contain additional items as required by
implementations.
