---
title: "Why Course Selection Pages Collapse at 9:00 AM
"
datePublished: 2026-03-01T13:58:06.266Z
cuid: cmm7teugo00ux2eoi0aqm9a6j
slug: why-course-selection-pages-collapse-at-9-00-am
cover: https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/b7d70717-f9a4-4eab-8b5c-4fea3a11e392.png

---

Course registration is a familiar source of anxiety for college students. Each semester, when enrolment opens, the same pattern repeats: pages hang on infinite loading screens, 503 “Service Unavailable” errors appear, and the system crashes without explanation. At this point, it almost feels expected.

At exactly 9:00 AM, thousands of students click the same URL within seconds of one another.

What happens next is not random failure. It is predictable behaviour at scale.

Let’s examine what occurs behind the scenes.

At 8:59 AM, the course selection page is quiet. A few students are logged in, waiting. The system is calm.

At 9:00 AM, thousands of students press “Enrol” within the same few seconds.

Imagine a lecture hall with only 60 seats and 2,000 students rushing through a single narrow door at once. No one is walking in an orderly line. Everyone is pushing forward at the same moment, trying to claim a seat before it disappears.

Some reach the door but cannot get through. Others are stuck behind them. A few manage to grab a seat. The rest step back, only to rush forward again immediately.

Now replace:

*   The door with a single database row that tracks available seats.
    
*   The 60 chairs with the remaining course slots.
    
*   The students with thousands of browser requests.
    
*   The pushing and retrying with repeated refreshes and repeated enrolment attempts.
    

From the outside, students see loading spinners and error messages.  
From the inside, the system is being flooded by simultaneous attempts to modify the exact same piece of data.

Nothing is broken individually. The collapse happens because everyone acts at the same time.

### The Thundering Herd Problem

The situation above is an example of the **thundering herd problem**.

A thundering herd occurs when a large number of clients wake up at the same time and compete for the same limited resource.

In course registration:

*   The shared trigger: 9:00 AM registration opening.
    
*   The shared resource: the same course seat counter.
    
*   The shared behaviour: immediate retries when enrolment fails.
    

At 9:00 AM:

1.  Thousands of students request the course page.
    
2.  All of them see limited seats available.
    
3.  Many attempt to enrol simultaneously.
    
4.  The system tries to update the same seat counter.
    
5.  Lock contention increases.
    
6.  Some requests fail or timeout.
    
7.  Students refresh and retry.
    
8.  Load increases further.
    

The system does not collapse because of traffic volume alone.

It collapses because thousands of requests converge on the same resource at the same moment.

That convergence is the thundering herd.  
  
**Let's visualise this:**

![](https://cdn.hashnode.com/uploads/covers/69a43d65a7428b958dbeccff/929b641d-f238-46b5-b2fe-47abdba0f21c.png align="center")

**Now, let's get a little technical for your interviews**  
  
The **thundering herd problem** is a concurrency issue where a large number of clients or threads are simultaneously awakened by a shared trigger and compete for the same limited resource. This sudden and synchronised access causes **resource contention**, **increased latency**, **lock queues**, and **retry amplification** (automatic or from clients). This can cascade into widespread performance degradation or system failure.