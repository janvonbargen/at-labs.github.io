---
title: Locations API
short: Getting started with the Locations API
layout: post
category: gettingstarted
date: 2016-08-15 16:00:00 +1200

---

The purpose of the Locations API is to publish geographical locations relevant to Auckland Transport.

**Format**

All API responses contain geo-location information in a standard format:

```json
{
   "id": "...",
   ...
   "latitude": ...,
   "longitude": ...
}
```

Depending on the type of location, further information is provided.

# Customer Service Centres

The customer service centre API, pretty much as the name suggests, lists available Auckland Transport service centres across Auckland, with contact details, opening hours, if HOP services are provided, etc.

# Parking

The parking API lists all Auckland Transport managed (i.e. not the ones operated by Wilson Parking and others) parking buildings and spaces, including facilities for mobility parking.

Note the response of this API call is rather large. Please [contact us]() if you are after something particular, like a search function.

# Points of Interest

== This is broken at the moment ==

# Scheduled Works

The scheduled works API, again as the name suggests, lists all scheduled works across the Auckland Transport managed road network (which btw excludes the motorways; these are NZTA-managed). The API returns additional geographical details, like if a particular area is affected, as well as planned start and end dates, contractor details, etc.

Note the response of this API call is rather large (the contractors are busy). Please [contact us]() if you are after something particular, like a search function.
