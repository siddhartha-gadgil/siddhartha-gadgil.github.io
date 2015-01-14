---
layout: post
title: "Differentiable Function Combinators"
date: 2015-01-02 15:24:10 +0530
comments: true
categories: Proving-Ground
---

## Basic functions and Combinators

We build differentiable functions for our basic learning system using some basic ones and combinators:

* compositions (in class)
* sums and scalar products of differentiable functions. (done)
* product of a real valued and a vector valued function - the subtlest case (done).
* identity function (done).
* projections on (V, W) (done)
* inclusions to (V, W) (dome)
* evaluation of a finite distribution at a point.
* atomic distribution as a function of weight.
* point-wise multiplication of a finite distribution by a given function.
* sum of a set of functions, with even the set depending on argument (done).
  * this can be interpreted as the sum of a fixed set of functions, but with all but finitely many  zero.
* repeated squaring $k$ times, with $k=0$ and $k<0$ cases (done).
* recursive definitions for families indexed by integers - generic given zero. Done (for vector spaces).

## Derived from these

* (optional) moves
* (optional) pairings
* linear combinations.

## Convenience code

* Have implicit conversion from
  * a type T implicitly depending on a linear structure on T to have methods ++ and *:
  * DiffbleFunction[V, V] to a class with a multiplication method **:

###To Do:

* Construct differentiable functions for Finite Distributions - atom, evaluate and point-wise product.
