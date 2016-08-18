---
title: Response and error handling
short: How to read responses, and what errors look like
layout: post
category: gettingstarted
date: 2016-08-15 20:00:00 +1200

---

The response format for all API calls is unified, and - silently brushing over potential bugs - all behave the same way.

The response contains a `status` field, which can either be "OK" or "Error". Successful calls will have the `response` field populated, and the `error` field set to null. For errors, it's the other way around. Makes sense?

The status codes for successful operations are the [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) in the 2xx range , errors are in the 5xx range. We got - at least - that one right.

The format for a successful call looks like this:

```json
{
  "status": "OK",
  "response": ...,
  "error": null
}
```

All API calls also support the [JSONP](https://en.wikipedia.org/wiki/JSONP) format, by adding the `callback` parameter to the URL. The response looks like this (assuming 'cb' as the callback value, and formatted for readability):

```javascript
/**/
typeof cb === 'function' && cb({
   "status":"OK",
   "response": ...,
   "error":null
});
```

If an error should occur, then the response will look like this:

```json
{
  "status": "Error",
  "response": null,
  "error": {
    "name": "...",
    "message": "..."
  }
}
```

Should be fairly self-explanatory.
