---
id: 7izbnov3vo8esfmctmtyjyi
title: Reactive Programming In Java (Course Notes)
desc: ''
updated: 1672820844976
created: 1672819245128
---

> These notes are based entirely off the course: ["Reactive Programming with Java"](https://www.youtube.com/watch?v=OiRx2pZskR0&list=PLqq-6Pq4lTTYPR2oH7kgElMYZhJd4vOGI&index=5)

The default programming module in java is known as the blocking programming module. We make a request, it takes time ot process, and then it responds. The alternative to this is what is known as Reactive Programming.

In Java, there are several frameworks that do this, namely:

- Reactor - has the maximum momentum of all the projects and is integrated with spring boot. A lot of cloud providers use it e.g. azure for their apis
- RxJava

A lot of people confuse reactive with async. Reactive programming doesn't imply it's async as it can by synchronous. A typical or traditional use cases of reactive programming are:

- user events
- I/O responses e.g. file is completed

Reactive programming = "react to something".

It's not exactly event driven programming.

Why do we care:

When we think about modern application development, there are things that stand out today:

1. high data scale
2. high usage scale e..g. lots of users, availability
3. cloud based costs - when servers were on prem it did not matter as much as cloud as inefficiencies can cost a lot

Before doing vertical or horizontal scaling, it's often important to optimize the code.

Given Java is non-blocking, Reactive programming can be seen commonly in JavaScript e.g. Node JS with Promises

Imagine a web application where you have a request from a user to retrieve preference:

```Java
@GetMaping("/users/{userId}")
public User getUserDetails(@PathVariable String userId) {
    User user = userService.getUser(userId);
    UserPreferences prefs = userPreferenceService.getPreferences(userId);
    user.setPreferences(prefs);
    return user;
}
```

This endpoint makes a call to two services which are run sequentially as opposed to in parallel i.e. `userService` call to `getUser` must complete before `getPreference` is triggered via the `userPreferenceService`. This becomes a problem when we consider the requests as spinning up new threads for each user. Given these two services are not dependant on each other, the longevity of the request takes longer and the result is more threads are alive at any given time than necessary. The issue is that we now have wasted resources/hardware.

Traditionally, Java developers don't think about concurrency as spring boot or mvc abstracts and handles it for them. When writing APIs, the mentality of the developer is that this code is handling a single request. Methods are written in a stateless way so each request does not touch another. The cost to this becomes:

1. sequential blocking operations
2. idling threads

Reactive programming is not the only solution. There are concurrency APIs in Java which can handle this. For example, doing the previous `getUserDetails` blocking section using concurrency APIs:

```Java
CompletableFuture<User> userAsync = CompletableFuture.supplyAsync(() -> userService.getUser(userId));
CompletableFuture<UserPreferences> userPreferencesAsync = CompletableFuture.supplyAsync(() -> userPreferenceService.getPreferences(userId));
// combine into a single future
CompletableFuture<Void> bothFutures = CompletableFuture.allOf(userAsync, userPreferencesAsync)
// block until they're ready
BothFutures.join();
// get the results and put into user
User user = userAsync.join();
// get the results and put into prefs
UserPreferences prefs = userPreferencesAsync.join();
user.setUserPreferences(prefs);
return user;
```

This works however it's still causing a thread to idle and that is due to `bothFutures.join()` which, although allows the two services to be called in parallel, still blocks until they are complete. The other problem is this that Future API like the example above is very messy and a lot to work with. This is what Reactive Programming aims to resolves.

The above example, using Reactive Programming, becomes:

```Java
public Mono<User> getUserDetails(@PathVariable String userId) {
    return userService.getUser(userId)
        .zipWith(userPreferencesService.getPreferences(userId))
        .map(tuple -> {
            User user = tuple.getT1();
            user.setUserPreferences(tuple.getT2);
            return user;
        })
}
```
