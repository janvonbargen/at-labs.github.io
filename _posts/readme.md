This directory contains all post-like content on ATLabs: Getting started, blog, etc. The implementation uses the `category` variable in the front matter (to use Jekyll's term) to determine where content should appear.

# Blog Content

Blog content works as expected. The order is determined by the date in the `date` variable. Sample front matter looks like this:

```yaml
---
title: Some title
short: Some description
layout: post
category: blog
date: 2016-08-11 17:00:00 +1200
---
```

Don't forget the timezone, or it'll show different dates, and perhaps ordering, between the online environment and your local one.

# Getting Started Content

Content for getting started works just like blog content, with a few gotchas. Different to the blog content, the order is determined by the `index` variable. The template also supports dividing the content into sections - determined by the `section` variable.

Sample front matter:

```yaml
---
title: Some API
short: Some description
layout: post
category: gettingstarted
section: api
index: 40
date: 2016-08-15 16:00:00 +1200
---
```

To add a new section it needs to be added to the /gettingstarted.html page:

```yaml
---
layout: gettingstarted
showtags: [general, api]
---
```

A further noteworthy extension is the introduction of information blocks (which render a block of text in a highlighted way). To use an information block it needs to be defined as a block attribute - inside of the markup:

```
Some information block text.
{: .warning}
```

The styles available are "info", "tip", "warning" and "note".
