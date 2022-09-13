---
title: "ZeroMQ"
date: 2022-09-03T02:00:56-04:00
# weight: 1
# aliases: ["/first"]
tags: ["ZeroMQ"]
author: "Kin"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

[[zeroMQ]]
[[MQ]]


ZeroMQ shouldnot be regarded as a pure socket library, it should be regarded as a messaging library, a "lightweight" message queue library.

[zmq_socket(3) - 0MQ Api (zeromq.org)](http://api.zeromq.org/3-2:zmq-socket)
[ZeroMQ | Socket API](https://zeromq.org/socket-api/)

## Key differences to conventional sockets

Generally speaking, conventional sockets present a _synchronous_ interface to either connection-oriented reliable byte streams (SOCK_STREAM), or connection-less unreliable datagrams (SOCK_DGRAM). In comparison, ØMQ sockets present an abstraction of an asynchronous _message queue_, with the exact queueing semantics depending on the socket type in use. Where conventional sockets transfer streams of bytes or discrete datagrams, ØMQ sockets transfer discrete _messages_.

ØMQ sockets being _asynchronous_ means that the timings of the physical connection setup and tear down, reconnect and effective delivery are transparent to the user and organized by ØMQ itself. Further, messages may be _queued_ in the event that a peer is unavailable to receive them.

Conventional sockets allow only strict one-to-one (two peers), many-to-one (many clients, one server), or in some cases one-to-many (multicast) relationships. With the exception of _ZMQ_PAIR_, ØMQ sockets may be connected **to multiple endpoints** using _zmq_connect()_, while simultaneously accepting incoming connections **from multiple endpoints** bound to the socket using _zmq_bind()_, thus allowing many-to-many relationships.

## Socket Types
不同通信pattern不同"socket"

ZeroMQ patterns are implemented by pairs of sockets with matching types.

### REQ-REP pattern
connects a set of clients to a set of services. This is a remote procedure call and task distribution pattern.

### PUB-SUB pattern
connects a set of publishers to a set of subscribers. This is a data distribution pattern.

### Pipeline pattern
connects nodes in a fan-out/fan-in pattern that can have multiple steps and loops. This is a parallel task distribution and collection pattern.

### Exclusive pair pattern
connects two sockets exclusively. This is a pattern for connecting two threads in a process, not to be confused with “normal” pairs of sockets.


[ØMQ - The Guide - ØMQ/2.2 - The Guide (zeromq.org)](http://zguide2.zeromq.org/py%3aall#toc53)


[Python Multiprocessing – ZeroMQ vs Queue | Tao te Tek (wordpress.com)](https://taotetek.wordpress.com/2011/02/03/python-multiprocessing-zeromq-vs-queue/)


### [Multithreading with ØMQ](http://zguide2.zeromq.org/py%3aall#Multithreading-with-MQ)

To make utterly perfect MT programs (and I mean that literally) **we don't need mutexes, locks, or any other form of inter-thread communication except messages sent across ØMQ sockets.**

By "perfect" MT programs I mean code that's easy to write and understand, that works with one design language in any programming language, and on any operating system, and that scales across any number of CPUs with zero wait states and no point of diminishing returns.

If you've spent years learning tricks to make your MT code work at all, let alone rapidly, with locks and semaphores and critical sections, you will be disgusted when you realize it was all for nothing. If there's one lesson we've learned from 30+ years of concurrent programming it is: _just don't share state_. It's like two drunkards trying to share a beer. It doesn't matter if they're good buddies. Sooner or later they're going to get into a fight. And the more drunkards you add to the table, the more they fight each other over the beer. The tragic majority of MT applications look like drunken bar fights.
