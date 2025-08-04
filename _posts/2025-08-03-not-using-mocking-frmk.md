---
layout: post
title: Why not using mocking framework
date: '2025-08-03T01:00:00.000-08:00'
author: Henri Tremblay
tags:
  - Pragmatism applied
---

Recently, I read an interesting [post](https://martinelli.ch/why-i-dont-use-mocking-frameworks-and-why-you-might-not-need-them-either/) by Simon Martinelli.
Simon is a smart man that I'm always happy to meet.
He was explaining why he tries to avoid mocking frameworks like EasyMock.

I thought it would be interesting to give my perspective on the topic.

# Prefer Pure Functions

Absolutely! 
The more pure functions you have in your codebase, the simpler testing becomes. 
No mocks needed - just straightforward, enjoyable testing.

# Do not mock the database

I agree completely. 
Instead of mocking databases, I "unit test" queries against real databases.
I frequently use [H2](https://www.h2database.com/html/main.html) but it can quickly shows its limitations.
The need of a real database through [TestContainers](https://www.testcontainers.org/) comes quickly.

# Do not mock value types

While Simon doesn't address this point, it's worth mentioning. 
Value types - classes that only hold data without behavior - shouldn't be mocked. 

Instead of this:

```java
record ValueHolder(Object value) {}

@Test
void test() {
  ValueHolder holder = mock(ValueHolder.class);
  when(holder.value()).thenReturn("x");
  // ...
}
```

Simply create the actual object:

```java
@Test
void test() {
  ValueHolder holder = new ValueHolder("x);
  // ...
}
```

# Time-based Code and Randomness

For time-dependent code, I prefer injecting a custom `java.time.Clock` implementation rather than using mocking frameworks.
I talk about it [here](2021-01-12-mocking-clock).

Similarly, for random behaviors, I create deterministic implementations. 
These concrete classes can be reused throughout the application's test suite.

# External Systems

Yes. Agreed. I mock. 
And, I will indeed try to mock or stub the REST endpoint rather than the client-side wrapper. 
Tools like [WireMock](https://wiremock.org/), Spring test facilities, or even the JDK's built-in `sun.net.httpserver.simpleserver.JWebServer` offer excellent solutions.

# What I do differently

This article wouldn't be interesting if I didn't disagree a bit.

The example extracts the price calculation to a `PriceCalculator` that you can then test thoroughly.
That's a classical separation of concern.
It does reduce the complexity of `addItem`.
Which then makes the test easier and removes the "I code my test against the implementation" effect.

But now that you are there, `OrderService` still has 4 dependencies: `OrderRepository`, `ProductRepository`, `CustomerRepository` and the newly created `PriceCalculator`.
We can ignore the latter because it's only performing calculations.
No mocking needed.

If you look at the [final code]( https://github.com/simasch/no-mocks), it uses Spring to inject the real implementations of the dependencies.
The test will then test everything in bulk.
I call that coarse-grained testing.

When deciding on test granularity, you have several valid options:
- Mock all dependencies if they're cumbersome to use directly
- Selectively mock some dependencies while using real implementations for others
- Test everything together as an integrated unit

Each approach has merit depending on your context.

# Conclusion

I disagree that applications are always so simple that you can test everything all the way down without mocking.
Sometime, you do need a good old mocking framework.

But Simon is making an excellent point.
You should always think about it before mocking all around until the test passes.
