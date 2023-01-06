---
id: aig5i0xuh2jq0ibzfq8sz22
title: Reactive Programming
desc: ''
updated: 1672820609674
created: 1672719811772
---

**Reactive Programming** is a design paradigm that is programming with asynchronous data streams. These data/event streams are observable and can be used to trigger "side-effects". A simple example of this is a click event. In an application we can fire effects upon the trigger of a user click by observing that event.

Data streams in reactive programming can be made up of any sequence of ongoing events ordered in time. These events are captured asynchronously and subscribed to // todo

## Reactive Manifesto

In 2013, a team of developers defined a set of core principals known as [the Reactive Manifesto](https://www.reactivemanifesto.org/) which describes what makes a system reactive. It prescribes that they must be:

- Responsive - provide rapid and consistent response times
- Resilient - stay responsive in the face of failure
- Elastic - stay responsive under varying workload by increasing/decreasing allocated resources
- Message Driven - asynchronous message passing to ensure lose coupling, isolation and location transparency.

- ![](/assets/images/2023-01-03-15-50-59.png)

## Why Use RP

#### References:

1. https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
