---
layout: post
title: "Differentiable Function Combinators"
date: 2015-01-02 15:24:10 +0530
comments: true
categories:
---

## Basic functions and Combinators

We build differentiable functions for our basic learning system using some basic ones and combinators:

* compositions
* sums and scalar products of differentiable functions.
* product of a real valued and a vector valued function - the subtlest case.
* identity function.
* projections on (V, W)
* inclusions to (V, W)
* evaluation of a finite distribution at a point.
* atomic distribution as a function of weight.
* sum of a set of functions, with even the set depending on weight.
  * this can be interpreted as the sum of a fixed set of functions, but with all but finitely many  zero.
* repeated squaring $k$ times, with $k=0$ and $k<0$ cases.
* recursive definitions for families indexed by integers.

## Derived from these

* (optional) moves
* (optional) pairings
* linear combinations.

## Convenience code

* Have implicit conversion from
  * a type T implicitly depending on a linear structure on T to have methods ++ and *:
  * DiffbleFunction[V, V] to a class with a multiplication method **:
