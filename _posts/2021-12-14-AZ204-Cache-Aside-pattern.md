---
layout: post
title:  "AZ204: Cache-Aside pattern"
categories: design-patten
tags:   azure az204 cache design-patten
---

# [Cache-Aside pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/cache-aside)

![](/images/2021-12-14-19-42-26.png)

### Considerations

- Lifetime of cached data
- Evicting data
- Priming the cache
- Consistency
- Local (in-memory) caching


### When to use this pattern

Use this pattern when:

- A cache doesn't provide native read-through and write-through operations.
- Resource demand is unpredictable. This pattern enables applications to load data on demand. It makes no assumptions about which data an application will require in advance.

This pattern might not be suitable:

- When the cached data set is static. If the data will fit into the available cache space, prime the cache with the data on startup and apply a policy that prevents the data from expiring.
- For caching session state information in a web application hosted in a web farm. In this environment, you should avoid introducing dependencies based on client-server affinity.


The order of the steps is important. Update the data store before removing the item from the cache.
