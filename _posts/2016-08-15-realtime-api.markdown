---
title: Realtime API
short: Getting started with the Realtime API
layout: post
category: gettingstarted
section: api
index: 20
date: 2016-08-15 16:00:00 +1200

---

The purpose of the Realtime API is to provide real-time data for all of Auckland's public transport modes: buses, trains and ferries.

{:refdef: .note}
At the time of writing, only real-time data from buses is available. The integration of real-time data is in progress, and ferries are planned.
{:refdef}

Most of the public transport data available follow the model of Google's Realtime Transit Specification (GTFS Realtime). A good place to start is the [GTFS Realtime Reference](https://developers.google.com/transit/gtfs-realtime/reference/) - this will tell you a lot about the domain, for entities, trip updates, vehicle positions and alerts (to name the top-level structural elements).

**Format**

The GTFS Realtime [specification](https://github.com/google/transit/blob/master/gtfs-realtime/proto/gtfs-realtime.proto), technically, uses Google's [Protocol Buffers](https://developers.google.com/protocol-buffers/) as its serialisation format. The Realtime API, by default, offers the same in JSON, but can also return protobuf if the `Accept` header is set to "application/x-protobuf".

**Naming Convention**

The API uses the convention described in the GTFS Realtime Specification - snake case - as its naming convention. For example

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

Same thing as for the GTFS API: You may not like the underscores, but this is the easiest if you want to re-use their documentation. Please don’t ask for camel/pascal/random case.

**Realtime**

At the moment, the feed is updated at least every 30 seconds. The GTFS Realtime specification provisions for, but does currently not allow incremental/differential updates - the GTFS Realtime API behaves to spec.

# Realtime API

The Realtime API provides the aggregated results of the *Trip Update API*, the *Vehicle Position API*, and the *Alert API* - please refer to the subsequent sections for further details.

The API, by default, returns all updates, but query parameters can be specified to filter by vehicles or trips.

# Trip Update API

The Trip Update API provides progress updates for all vehicles within Auckland Transport's network. 

The API, by default, returns all updates, but query parameters can be specified to filter by vehicles or trips.

## Trip Descriptor

> A descriptor that identifies an instance of a GTFS trip, or all instances of a trip along a route.

The `trip_id` and `route_id` refer to the trips and routes published through the GTFS API. Please note the special management of routes, explained in the [GTFS API](../gtfs-api/).

The fields `direction_id`, as well as `start_time` and `start_date` are not set. They can be retrieved from the GTFS API if required.

The `schedule_relationship` is always set to 0 (SCHEDULED). The backend system does not process added, unscheduled or cancelled trips.

## Vehicle Descriptor

> Identification information for the vehicle performing the trip.

Currently, only the `id` is set. The id is provided by AVL (automatic vehicle location) devices installed on buses, trains and ferries. It is, as per specs, strictly a string, although it currently, for buses, looks numerical (e.g. for trains it looks very different, and even has spaces in it). The id is just that: an identifier for a vehicle. As vehicles can be used for different routes, or even swapped (in case a vehicle breaks down, for example), it should not be used to deduct routes or similar.

The id (& format) may also change in future - so please also don't try to deduct anything else from it.

## Stop Time Event

> Timing information for a single predicted event (either arrival or departure).

Both `delay` and `time` fields are set. Note that the delay can be, and, particularly for buses, reasonably often is, negative.

The `uncertainty` field is currently omitted. According to the specs it probably should be set, given the API is returning the `delay`.

## Stop Time Update

> Realtime update for arrival and/or departure events for a given stop on a trip. Updates can be supplied for both past and future events.

The fields `stop_sequence` and `stop_id` or both set. The arrival and departure *stop time event*s are populated when the vehicle either enters a stop location (arrival) or exits a stop location (departure).

The `schedule_relationship` is always set to 0 (SCHEDULED) - the backend system doesn't process skipped stops.

# Vehicle Position API

TODO

# Alert API

TODO
