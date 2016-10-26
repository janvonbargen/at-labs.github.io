---
title: "New: Notifications API"
short: Getting started with the Notifications API
layout: post
category: gettingstarted
section: api
index: 50
date: 2016-10-12 11:00:00 +1200

---

The Notifications API publishes event data that has an impact on travel in general - public transport, and Auckland's road network.

**Format**

The format for notifications is currently derived directly from the backend systems - and as such not (yet) normalised. We don't know yet how these APIs are going to be used, therefore making up a structure and format seemed premature. Please let us know your suggestions.
{: .note}

# Notifications API

The Notifications API provides the aggregated results of all categories of the *Notifications by Category API*.

## Notifications by Category

Notifications are issued in several categories:

* moved_stop (Stops not serviced)
* road (Roadworks)
* events (Public events like sports or concerts)
* real_time
* highway (Highway maintenance)

All notifications returned as part of this API (The *Notifications by Stop API* has a different format) follow a base format:

```json
{
   "time": "...",
   "data": [
      {
         "id": ...,
         "orderingDate": ...,
         "type": ... (category),
         "title": ...,
         "description": ...,
         ...
      },
      ...
   ]
}
```

Depending on the category, other fields are returned; for example, a road-type event will include `subtype`, `startDate` and `endDate`, a moved stop-event will include `stopId`, etc.

The `id` field is unique within the dataset returned. However, it is not guaranteed to be unique over time. Don't use it to synchronise data.

Special note: The highway-type event returns `expectedResolution`, which is a string value - not a date. The value can be something like "Until further notice", or a date like "26/10/2016 06:00"; note that the date is not in ISO 8601/RFC 3339. Care should be taken when attempting to parse the date.

## Notifications by Stop

The *Notifications by Stop API* returns details about new stops to be used if a particular public transport stop is currently not serviced/used. Similar data us published under the "moved_stops" category of the *Notifications by Category API*; this API adds the alternative stops to be used.

The origin of the data published is different to the origin of the *Notifications by Category API*. The data usually aligns, but it doesn't always have to be the case.
{:.note}

The format of the data returned looks like this:

```json
[
   {
      "attributes": {
         "OBJECTID": ...,
         "AFFECTEDSTOPID": ...,
         "AFFECTEDROUTE": ...,
         "NEWSTOPID": ...,
         "NEWSTOPNAME": ...,
         "NEWSTOPEXISTINGROUTE": ...,
         "NEWSTOPSTATUS": ...,
         "NEWSTOPNOTE": ...
      }
   },
   ...
]
```

The API may return multiple "new" stops for an affected existing stop. Multiple stops indicate that some services for the stop have been re-routed to one "new" stop, and other services are routed to the other "new" stop(s).

The `AFFECTEDROUTE` field contains a comma-separated list of, well, affected bus services. Note that the affected services always includes all routes from the affected stop, not the services routed through the new stop. In other words, the list of affected routes is always the same, even if multiple new stops are returned. 

The GTFS *Routes By Stop* API can be used to determine which routes exactly are serviced by which "new" stop.
