---
title: Realtime API
short: Getting started with the Realtime API
layout: post
category: gettingstarted
section: api
index: 20
date: 2016-08-15 16:00:00 +1200

---

The purpose of the Realtime API is to provide real-time data for each of Auckland's public transport modes: buses, trains and ferries.

At the time of writing, only real-time data for buses is available. Integration of real-time train data is in progress, and ferries are planned.
{: .note}

Most of the public transport data available follow the model of Google's Realtime Transit Specification (GTFS Realtime). A good place to start is the [GTFS Realtime Reference](https://developers.google.com/transit/gtfs-realtime/reference/). This will tell you a lot about the domain: entities, trip updates, vehicle positions and alerts (to name the top-level structural elements).

**Format**

The GTFS Realtime [specification](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto) technically uses Google's [Protocol Buffers](https://developers.google.com/protocol-buffers/) as its serialisation format. The Realtime API offers the same in JSON, by default, but can also return protobuf if the `Accept` header is set to "application/x-protobuf".

**Naming Convention**

The API uses the convention described in the GTFS Realtime Specification (snake case) as its naming convention. For example

```
required FeedHeader header = 1;
repeated FeedEntity entity = 2;
```

becomes

```json
{
   "header": {
      ...
   },
   "entity": [
      ...
   ]
   ...
}
```

The same applies to the GTFS API: You may not like the underscores, but if you want to re-use their documentation it is best to follow their convention. Please don’t ask for camel/pascal/random case.

**Realtime**

At the moment, the feed is updated at least every 30 seconds. The GTFS Realtime specification provisions for, but does not currently allow incremental/differential updates. i.e. The GTFS Realtime API behaves to spec.

# Realtime API

The Realtime API provides the aggregated results of the *Trip Update API* (vehicle progress along a trip), the *Vehicle Position API* (positioning information for a vehicle), and the *Alert API* (notification of incidents) - please refer to the subsequent sections for further details.

The API returns all updates by default, but query parameters can be specified to filter by vehicles or trips.

# Trip Update API

The Trip Update API provides progress updates for all vehicles along a trip within Auckland Transport's network.

The API returns all updates by default, but query parameters can be specified to filter by vehicles or trips.

## Trip Update

> Real-time update on vehicle progress along a trip.

According to the GTFS Realtime specification, the Stop Time Update has a cardinality of "repeated". The JSON feed only allows a single update - which is incorrect. Future versions of this API should fix this.
{: .warning}

The fields/structures `trip`, `vehicle` and `stop_time_update`, as well as `timestamp`, are provided. The optional and experimental `delay` field is omitted.

## Trip Descriptor

> A descriptor that identifies an instance of a GTFS trip, or all instances of a trip along a route.

The `trip_id` and `route_id` refer to the trips and routes published through the GTFS API. Please note the particular management of routes, explained in the [GTFS API](../gtfs-api/).

The fields `direction_id`, as well as `start_time` and `start_date` are not set. They can be retrieved from the GTFS API if required.

The `schedule_relationship` is always set to 0 (SCHEDULED). The backend system does not process added, unscheduled or cancelled trips.

## Vehicle Descriptor

> Identification information for the vehicle performing the trip.

Currently, only the `id` is set. The id is provided by AVL (automatic vehicle location) devices installed on buses, trains and ferries. It is a string, as per specs, although it currently looks numerical for buses. For trains, it looks very different, and even includes spaces. 

The id is just that: a vehicle identifier. As vehicles can be used for different routes, or even swapped (in the case of a vehicle break down, for example), the id should not be used to deduce routes or similar. Secondly, the id (& format) may change in future, so please don't try to read anything else into it.

## Stop Time Event

> Timing information for a single predicted event (either arrival or departure).

Both `delay` and `time` fields are set. Note that the delay can be negative reasonably often, particularly for buses.

The `uncertainty` field is currently omitted. According to the specs it probably should be set, given the API is returning the `delay`.

## Stop Time Update

> Realtime update for arrival and departure events for a given stop on a trip. Updates can be supplied for both past and future events.

The fields `stop_sequence` and `stop_id` are both set. The arrival and departure *stop time event*s are populated when the vehicle enters a stop location (arrival) or exits a stop location (departure).

The `schedule_relationship` is always set to 0 (SCHEDULED) - the backend system does not process skipped stops.

# Vehicle Position API

The Vehicle Position API provided real-time updates for all vehicles in the Auckland Transport network.

## Vehicle Position

> Real-time positioning information for a given vehicle.

The fields/structures `trip`, `vehicle` and `position`, as well as `timestamp`, are provided. All other fields, like `stop_id`, are omitted.

## Trip Descriptor

> A descriptor that identifies an instance of a GTFS trip, or all instances of a trip along a route.

Please see the *Trip Descriptor* section, part of the *Trip Update API*, above.

For trip descriptors in the context of the vehicle position messages, the field `start_time` is included.
 
## Vehicle Descriptor

> Identification information for the vehicle performing the trip.

Please see the *Vehicle Descriptor* section, part of the *Trip Update API*, above.

## Position

> The geographic position of a vehicle.

Both `latitude` and `longitude`, as per the GTFS Realtime specification, are included. The `bearing` is only provided if the AVL device on the bus provides it.

The `odometer` and `speed` fields are currently not provided.

# Alert API

The Alert API provides notifications for incidents across Auckland Transport's public transport network.

## Alert

> A notification of an incident in the public transit network.

All fields for the alert entity are populated - including optional fields. English is the only translation available for the `url`, `header_text` and `description_text` fields.

The `active_period` currently sets the `start` or `end` to "null", and doesn't omit them as per the GTFS Realtime specification. This may change in future.
{: .warning}

The alert entity also provides two extension elements:

1. `is_show_header`: Indicates if the `header_text` should be displayed
2. `level`: This field indicates the severity of the incident - similar to log entries. The value can be either of "Info", "Warning" or "Critical".

These extensions are used internally and don't form part of the API contract. While they can be used, care should be taken as future versions of the API may refactor or remove them.
{: .note}
