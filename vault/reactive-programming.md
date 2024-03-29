---
id: aig5i0xuh2jq0ibzfq8sz22
title: Reactive Programming
desc: ""
updated: 1673861535837
created: 1672719811772
---

**Reactive Programming** is a design paradigm that is programming with asynchronous data streams. These data/event streams are observable and can be used to trigger "side-effects". A simple example of this is a click event. In an application we can fire effects upon the trigger of a user click by observing that event.

[The Gang of Four](https://en.wikipedia.org/wiki/Design_Patterns) famously defined two patterns that became fundamental to the focus of reactive programming:

- Iterator - the consumer pulls data out of the producer (imperative - consumer is in control)
- Observer - a way for a producer to give the consumer one item at a time (declarative - producer is in control)

The book misses what is the correspondence between these two patterns: The iterator pattern allows you to pull data out and the producer lets the consumer know when there is no more data AND if an error occurred. These two semantics were left out of the observer pattern by the GoF.

Reactive programming is about unifying the observable type with the iterator type and therefore produces this new type known as the "observable". The benefit of this is now we can react in the way we do to static data to dynamic events in real time.

By adding these two completion semantics of the two patterns, we can now use all the ways we know when it comes to how to transform streams of data that we can pull on the streams that we pull e.g. map, filter, reduce, zip, merge etc.

The advantage of this is now event data does not need to be stored anywhere when it comes to processing. Meaning we don't need to have big data stores that we pull from in order to process data - we can take the data as it arrives, process it and be on our way. We allocate less memory this way. If you can write a sql query to transform data in a temporary table somewhere, you can now write a sql query over live data. It's all about serving the consumer faster. [^3]

#### References:

1.  https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
2.  https://dev.to/this-is-learning/what-the-hell-is-reactive-programming-anyway-31p5
3.  [Reactive Programming Overview (Jafar Husain from Netflix)](https://www.youtube.com/watch?v=-8Y1-lE6NSA)

[^1]: Quote from https://dev.to/this-is-learning/what-the-hell-is-reactive-programming-anyway-31p5
