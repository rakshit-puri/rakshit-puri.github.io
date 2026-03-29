---
title: "When Caches Fail"
datePublished: 2026-03-08T18:25:19.705Z
cuid: cmmi31glz01cu2eq92nzda80t
slug: cache-strategies
cover: https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/f54d838c-d3be-450b-8ebb-0d5fb2334174.png

---

In the previous blog, [Why Course Selection Pages Collapse at 9:00 AM](https://rp1.hashnode.dev/why-course-selection-pages-collapse-at-9-00-am), we discussed the **Thundering Herd Problem**, where a sudden surge of requests overwhelms a shared resource and can destabilize a system. One common mitigation technique is introducing a **cache layer** to reduce the load on the database.

At first glance, caching appears to solve the problem. Frequently requested data is served directly from memory, which avoids repeated database queries and significantly reduces latency.

However, caching introduces its own challenges.

When a cache entry expires or is missing, every request that needs that data attempts to fetch it from the database. If many users request the same data at the same time, the system experiences a burst of **cache misses**, causing multiple identical database queries.

This situation can recreate the same spike that caching was meant to prevent.

Real-world systems often encounter this pattern during high traffic events. For example:

*   During an **IPL match**, millions of users repeatedly refresh score updates on platforms like CricBuzz.
    
*   When a **new show releases on Netflix**, many users simultaneously load the same metadata, thumbnails, and descriptions.
    
*   During a **major e-commerce sale**, thousands of users request the same product information at once.
    
    In these situations, a simple cache with a fixed expiration time is not sufficient. If many cache entries expire at roughly the same moment due to same **time-to-live (TTL)** configuration, the resulting surge of cache misses can place enormous pressure on the database.
    
    The following sections examine techniques used in distributed systems to prevent these spikes and make caching resilient at scale.
    

* * *

## How to Prevent Cache Stampedes?

### **TTL Jitter (Randomized Expiration)**

Instead of a fixed expiration time, add randomness.  
**Example:** TTL = 600 seconds + random(0–60)

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/036f9022-f07e-4106-a786-a92088057398.png align="center")

**Effect:**

*   Cache entries expire at different times.
    
*   Requests to rebuild cache are spread out.
    
*   Prevents synchronized database spikes.
    

By adding a small amount of random noise (jitter) to each TTL, the expirations are staggered. While the same total number of requests eventually hit the database, they are spread across a wider time window, keeping the system load within safe limits.

### **MUTEX Locking**

Only **one request** is allowed to rebuild the cache.

**Purpose:**

*   Prevents thousands of identical DB queries.
    
*   Eliminates duplicate recomputation.
    

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/891535f8-fcd2-4657-aa6d-79336009f577.png align="center")

### **Stale-While-Revalidate**

  
Serve slightly stale data while refreshing the cache **asynchronously**.  
This technique is used in **Content Delivery Networks (CDNs)** and large web systems. This offers low latency and the request are not blocked. This is in contrast with Mutex locking technique we discussed above.

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/219d7f91-7f21-4d87-ac9c-5cde0c419598.png align="center")

### **Probability Early Expiration/Computation**

Refresh some cache entries **before TTL actually expires**.

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/c6a571ca-a15b-4edd-a869-051bf77d1af5.png align="center")

### **Cache Pre-warming**

Load popular data into cache before expected traffic spikes.

Example:

*   Product launches
    
*   Ticket sales
    
*   Streaming premieres
    

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/621848ac-8d8a-48e8-a244-f69ad9aa80c9.png align="center")

* * *

Each technique balances freshness, latency, and consistency differently. In production, the systems rarely rely on a single strategy. They combine several of these mechanisms to keep cache regeneration smooth and prevent database overload.

A resilient caching system is not defined only by how fast it serves cache hits, but by how gracefully it handles cache misses under real-world traffic conditions. An example is shown below.

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/99fd3e8f-d57a-4d61-b2c2-8f8c063bcc46.png align="center")

* * *

These scenarios rarely appear during testing. With four or five users, the system behaves perfectly and the cache seems flawless. The real problems only surface at scale, when thousands or millions of requests arrive at the same time. The goal of these strategies is to prepare systems for that inevitable moment.
