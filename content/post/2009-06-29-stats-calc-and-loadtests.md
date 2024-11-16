---
author: Jesse Morgan
categories:
- Uncategorized
date: "2009-06-29T13:49:56Z"
guid: http://morgajel.net/?p=517
id: 517
title: Stats, Calc and Loadtests
url: /2009/06/29/517
views:
- "55"
---

Preface: Kids, PAY ATTENTION IN MATH CLASSES. YOU NEED IT.

So I’ve been spending a lot of time at work doing loadtests, and one question that keeps coming back is “what metric do we want to use to gauge progress?” We’ve been going with a simple throughput (pages/second), but that doesn’t take into account errors, average times, or samplecounts.

You can have 30 pages/second and have the following conditions:

- average page takes 10 seconds to load (300 threads over a 10 second period)
- 50% error rate (pages that spit out a 500 error can return REALLY fast)
- 30 threads, 1 second page load

That number doesn’t tell the whole story. There’s also the number of samples, overall error rate, and average, median and 90th percentile page load times.

So how do you come up with a number that covers all of that?

<div>![](http://morgajel.com/equation.png)</div>This is my current (unweighted) equation.

The short version is ‘the higher the better’. The longer version is:

- higher samples good
- higher throughput good
- high page loads bad
- errors penalize score

Missing from this equation:

- proper weighting
- KB/sec
- Min and Max times
- a unit describing it’s awesomeness

So that’s what I’ve done today…