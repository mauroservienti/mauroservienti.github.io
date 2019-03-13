---
typora-copy-images-to: ..\img\posts\there-is-no-such-a-thing-as-cross-services-composition
typora-root-url: ..
layout: post
header_image: /img/posts/there-is-no-such-a-thing-as-cross-services-composition/header.jpg
title: "There is no such a thing as cross Services ViewModel Composition"
author: Mauro Servienti
synopsis: "At a first look it might sound reasonable to use ViewModel Composition to allow services to talk to each other. Why not allowing services to share complex data structure composed at runtime? Let me put it simple: you don't want a distributed monolith!"
tags:
- SOA
- Services ViewModel Composition
---

In fact there isn't such a thing as cross services data sharing at all. Lately I started a series of articles about Service ViewModel Composition, I talked about what problem is Services ViewModel Composition designed to solve in [What is Services ViewModel Composition, again?](https://milestone.topics.it/2019/02/06/what-is-services-viewmodel-composition-again.html)". How is Single Item Composition designed in [The Services ViewModel Composition maze](https://milestone.topics.it/2019/02/20/viewmodel-composition-maze.html). How is ViewModels Lists Composition designed in [Into the darkness of ViewModels Lists Composition](https://milestone.topics.it/2019/02/28/into-the-darkness-of-viewmodel-lists-composition.html). To finally show some code in [ViewModel Composition: show me the code!](https://milestone.topics.it/2019/03/06/viewmodel-composition-show-me-the-code.html).

## Share all the things!

At a first look it might sound reasonable to use the ViewModel Composition approach to allow services to talk to each other. If `Service X` needs some information that are stored in `Service B` and `Service H` why not allowing `X` to go to some sort of internal `Composition Engine` where requests handlers from `B` and `H` could be deployed to solve the exact problem `H` has...? Thank you, but no, thank you.

> As my Dutch colleague [Dennis van der Stelt](https://twitter.com/dvdstelt) would say "Autonomous microservices don't share data. Period.", that by the way is also the title of a talk he's going to give this month at [NDC Copenhagen, check it out](https://ndccopenhagen.com/talk/autonomous-microservices-dont-share-data-period/)!

Even if it's technically possible to build such a thing, if we don't want to end up with a "wonderful" distributed monolith we probably want to avoid it.

## Puff, autonomy is gone

Regardless of whatever method we use to share data across service boundaries we'll end up with services that are not autonomous. Autonomous services can evolve independently from each other, not autonomous ones cannot. They are coupled, they won't be able to evolve without breaking each other, causing evolution to stop or to cause friction at development time, deployment time, and run time. But there is more...

## The snowball effect

As soon as we have services querying each other for information we're immediately introducing temporal coupling. Whatever problem the queried service has it's propagated to the querying service. Is it down? The querying service cannot work. Is it slow? The querying service might timeout or fail. And so on, you probably get the point.

## Conclusion

As soon as we allow cross services data sharing, for example by allowing services to request data from another service, we end up with a the worst of 2 worlds: a monolith suddenly turns into a distributed monoliths with all distributed system problems on top of a monolithic one. The system immediately loses to key aspects: 

- Services are not autonomous anymore
- Services are temporally coupled

Despite being technically possible, just don't do it. If you feel the need go back to the drawing board, it's very likely that services boundaries are wrong. Be careful, what at the beginning seems a small harmless snowball can easily turn into an avalanche that halts the entire system.