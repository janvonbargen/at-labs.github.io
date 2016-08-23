This directory contains all post-like content on AT Labs: Getting started, blog, etc. The implementation uses the `category` variable in the front matter (to use Jekyll's term) to determine where content should appear.

The order is determined by the date in the `date` variable, also in the front matter.

# Blog Content

Blog content works as expected. Sample front matter looks like this:

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

Content for getting started works just like blog content, with a few gotchas. Just as blog content, the order is determined by the date. So we invent dates to achieve a particular order. The key date is 2016-08-15 - which is random; it is the date I started writing getting-started documentation.

All getting started post "files" have the file name `2016-08-15-[title].markdown`. The API related posts have the `date` variable set to `2016-08-15 20:00:00 +1200`, the more generic posts have it set to `2016-08-15 22:00:00 +1200` - a bit of a hack, I know.

Sample front matter:

```yaml
---
title: Some API
short: Some description
layout: post
category: gettingstarted
date: 2016-08-15 16:00:00 +1200
---
```

TODO: Find a better way.
