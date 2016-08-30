---
title: Authorisation
short: How to authorise for the AT APIs
layout: post
category: gettingstarted
section: general
index: 20
date: 2016-08-15 22:00:00 +1200

---

As explained in the [registration](../registration/) post, once you register for an API you'll receive two keys: A primary and a secondary key. You can use either, they are there to allow rolling updates and temporary access.

The key must be supplied in an HTTP header called `Ocp-Apim-Subscription-Key`.

For example:

**Groovy**

```groovy
def key = 'xxx'

def gtfs = new RESTClient('https://api.at.govt.nz/v2/gtfs/')
gtfs.defaultRequestHeaders.'Ocp-Apim-Subscription-Key' = key

gtfs.get(path: '...')
```

...