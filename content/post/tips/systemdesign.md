---
title: systemdesign
date: 2019-12-02T18:24:23+08:00
lastmod: 2019-12-02T18:24:23+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## System Design Interview Tips

## Clarify the constraints and identify the user cases
> Spend a few minutes questioning the interviewer and agreeing on the scope of the system.User cases indicate the main functions of the system, and constraints list the scale of the system such as requests per second, requests types, data written per second, data read per second.

## High-level architecture design
> Sketch the important components and the connections between them, but don't go into some details. Usually, a scalable system includes webserver (load balancer), service (service partition), database (primary/secondary database cluster plug cache).

## Component design
> For each component, you need to write the specific APIs for each component. You may need to finish the detailed OOD design for a particular function. You may also need to design the database schema for the database.

## Focus on thought process
> What we actually care about is the thought process behind your design choices.
> We need people we can trust to do the right thing without a lot of supervision—people who can own large projects and take them consistently in the right direction. Invariably, this means being able to communicate effectively with the people around you.

## Topics you should be familar with...
> Concurrency, Networking, Abstraction, Real-World Performance, Estimation,Availability and Reliability, and more

## How to prepare
> Do mock design sessions. Design interviews are similar to actual design sessions, so getting better at one will make you better at the other.
> Work on an actual system. Contribute to OSS or build something with a friend.
> Do back-of-the-envelope calculations for something you’re building and then write micro-benchmarks to verify them.
> Dig into the performance characteristics of an open source system. 
> Learn how databases and operating systems work under the hood.

## Relax and be creative
