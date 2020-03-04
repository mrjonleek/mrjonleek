---
title: "Coding With Quarkus &mdash; Part I"
date: 2020-02-13 00:00:00
description: Historically I've avoided Java and Java based frameworks because of their generally combersome develpoment experience and resource intensive deployments. Quarkus is changing my opinion.
featured_image: '/images/blog/quarkus.png'
---


## Quick Prologue
Recently a colleague invited me to join him as a co-speaker at a Java Users’ Group in San Diego, CA. on the topic of Quarkus. Although I’ve been a developer for a long time, I’d never looked to Java as a technology solution because other languages were, or at least seemed, more accessible. But, Quarkus is changing my opinion.

In this first of only a few posts on learning Quarkus, I’ll cover some of the reasons why and how Quarkus is an emerging Java framework you should consider for future development.

The term “Cloud-native”, coined by Netflix and made popular by Pivotal, is used in this series and it’s important to understand what that means. In its simplest form “cloud-native” refers to the deployment of applications natively in containerized environments like Kubernetes and OpenShift. A more in-depth definition would include concepts of multi-cloud, microservices, CI/CD and DevOps.

The article ["What are cloud-native applications?"](https://www.redhat.com/en/topics/cloud-native-apps) on the Red Hat website expands on this concept.

## What Quarkus Is
The traditional client-server “monolithic” architecture is rapidly being replaced by modern microservices, reactive applications, message-driven, and serverless design patterns. Quarkus is the answer to modern development paradigms for Java<sup><small>TM</small></sup> developers.

Quarkus is “Supersonic, Subatomic, Java” and claims to be “A Kubernetes Native Java stack tailored for OpenJDK HotSpot and GraalVM, crafted from the best of breed Java libraries and standards.”

At the time of this writing, Quarkus is made of some 90+ community projects most notably including [Eclipse Vert.x](https://vertx.io/), [Eclipse MicroProfile](https://microprofile.io/), [Hibernate](https://hibernate.org/), [Netty](https://netty.io/), [RestEasy](https://resteasy.github.io/), [Apache Camel](https://camel.apache.org/), [and many more](https://code.quarkus.io/). If you’re reading this as a Java developer then you’re likely familiar with many of these projects.

Quarkus describes the four key benefits it brings to the cloud-native community as **container first focused**, **unifying of imperative &amp; reactive design patterns**, **enhancing developer joy**, and **incorporating best of breed libraries and standards**.

### What is "Container First"?
Container first means that Quarkus is designed and optimized for containerized environments. Quarkus can compile applications to native code via the GraalVM or serve applications via the HotSpot JVM and with either of these, you’ll experience incredibly low RSS memory consumption and blazing fast boot times. The image below illustrates the footprint and initial response time of Quarkus compiled natively and hosted within the HotSpot JVM compared to a traditional cloud-native Java stack.

![Quarkus benchmark metrics comparted to a traditional Cloud-native stack.](/images/blog/quarkus_metrics_graphic_bootmem_wide.png)

[^1] <sup><small>image source</small></sup>

[^1]:_image source: https://quarkus.io/_

The metrics above clearly indicate a much smaller memory footprint and response time with each at a fraction of that compared to the traditional stack. When evaluating a typical `REST/CRUD` application the natively compiled Quarkus application consumes 28 MB RSS memory compared to 209 MB on the traditional stack with a response time of 0.016 seconds again compared to 9.5 seconds on the traditional application.

### Unifying Imperative &amp; Reactive
Imperative and reactive models are commonly associated with blocking and non-blocking design patterns respectively. Quarkus allows developers to implement the `imperative` and `reactive` models, interchangeably. This is unique in that it provides developers a way to embrace a container native framework while they continue to evolve their skills for modern development paradigms. This is accomplished by allowing developers to inject the `EventBus` or `Vert.x` context.

#### Imperative Model Snippet
The snippet below is a valid Quarkus implementation of the `imperative` design pattern, your traditional linear execution of statements.

```java
@Inject
SayService say;

@GET
@Produces(MediaType.TEXT_PLAIN)
public String hello() {
    return say.hello();
}
```

### Reactive Model Snippet
This snippet is a valid Quarkus implementation of the `reactive` design pattern to implement asynchronous non-blocking requests like streams, for instance.

```java
@Inject @Channel("kafka")
Publisher<String> reactiveSay;

@GET
@Produces(MediaType.SERVER_SENT_EVENTS)
public Publisher<String> stream() {
    return reactiveSay;
}
```

## Developer Joy
It’s not too often we think about development frameworks in terms of developer joy. Quarkus took the opportunity to simplify our lives a bit in designing this framework by unifying configuration, providing **live reload** capabilities and streamlining code for 80% of common use cases.

Unified configuration means just that: all your configurations in one place. One of the things that caught my attention is the live reload feature. Other languages and frameworks have had this for years and now within Quarkus developers can make changes locally and instantly have access to them in the browser.

![Quarkus live-reload, it's supersonic Java!](/images/blog/quarkus-livereload-wtf.png)

## Best of Breed Libraries &amp; Standards
As I mentioned at the beginning of this article Quarkus provides a cohesive, fun to use, full-stack framework by leveraging a growing list of over fifty best-of-breed libraries that you love and use. All wired on a standard backbone.

Head on over to the [Quarkus Extensions](https://quarkus.io/vision/standards) library to learn more.

## What Quarkus Isn't
I described Quarkus as the answer to modern development paradigms. **Quarkus is not intended to replace the _one app to rule them all_ patterns**. While, technically speaking, you can build a monolith if you so desire, the framework is not optimized or intended for these use cases. And, frankly, you probably want to start migrating away from this model anyway.

## Conclusion
Posts to follow will dive deeper and introduce us to the more technical aspects of Quarkus with hands-on examples of imperative and reactive implementations, live reload, configuration and build parameters so stay tuned.

If you're interested in getting involved in the project you can follow the [quarkus-dev Goolge Group](https://groups.google.com/d/forum/quarkus-dev) and check-out the code on [GitHub](https://github.com/quarkusio/quarkus).
