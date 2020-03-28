---
layout: post
published: true
title: "Studying linear algebra [Chapter 3]"
date: 2020-03-28 22:30
comments: true
author: BL
slides: simple
categories: architecture resque redis bus
---

{% slide %}
## Resque Bus
### Brian Leonard
##### TaskRabbit
##### 02/17/2014
{% notes %}
Hi, my name is Brian Leonard and I'm the Chief Architect and Technical Cofounder of TaskRabbit. TaskRabbit is a marketplace where neighbors help neighbors get things done.
At TaskRabbit we are using Redis and Resque to do all of our background processing. We've also gone one step further to use these tools to create an asynchronous message bus system that we call Resque Bus.
{% slide %}
### Redis
* Key/Value Store
* Atomic Operations
![Redis Logo](http://www.bleonard.com/images/posts/resque-bus-presentation/redis-logo.jpeg)
{% notes %}
I don't have to tell you guys about Redis, but for our purposes the main point is that we can store stuff in it and the operations are atomic.
{% slide %}
## Thanks!
Questions?
{% endslide %}
