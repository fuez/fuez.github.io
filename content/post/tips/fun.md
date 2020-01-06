---
title: "fun"
date: 2019-06-26T18:48:29+08:00
lastmod: 2019-06-26T18:48:29+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

 for fun

## We know that the way to get the last 1 bit from an integer value `v` is `v & (-v)`, do you know how to represent a negative value in binary form?

This [blog](https://www.calvin.edu/academic/rit/webBook/chapter5/negative.htm) explains that, today we reprent negative in so called *two's complement* scheme. So the key to understand this is to understand what is *two's complement*, this [wiki page](https://en.wikipedia.org/wiki/Two%27s_complement) tells detais about this.
> The two's complement of an N-bit number is defined as its complement with respect to 2N. For instance, for the three-bit number 010, the two's complement is 110, because 010 + 110 = 1000.

From the above statement, I understand why `v&(-v)` is the last 1 bit in v or -v, since in that way, when v + (-v) can carry over until `2<<N`
