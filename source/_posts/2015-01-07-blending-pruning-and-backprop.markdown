---
layout: post
title: "blending pruning and backprop"
date: 2015-01-07 10:25:14 +0530
comments: true
categories: Proving-Ground
---

## The problem

* One way to evolve a system is the _genetic algorithm_ style, where we consider the fitness of the _final_ elements and prune based on this. This however has trivial credit assignment.
* At the other extreme is a pure back-propogation. This has nice credit assignment, but done directly the support never changes.

## Solution

* Firstly, there must be an identity term in the evolution, so credit _can_ go to objects just persisting.
* Note that while the result of a big sum does not depend on terms that are zero, specifically $\mu(v)f$ with $\mu(v) = 0$, the gradient still depends on such terms. Specifically we can get a flow back to add weight to $v$  
* We _decouple_ the big sum component, so that when we apply to an argument which is a distribution, we do not just take a set of functions depending on that distribution. We can instead take a specified support.
* We take a big sum over all generated elements, to allow some of them to acquire weight, while computing the gradient.
* We should periodically prune by weight, but only after flowing for long enough to allow for picking up weight.

## Code

* We consider differentiable functions that also depend on a subset of V, by taking linear combinations including bigsums that are the _union_ of the support of a distribution with the given subset.
* For propagation, we just take an empty subset of V.
* Once we have the image, we take its (essential) support as the given subset of V. We recompute the differentiable function. Note that the value is unchanged.
* We use the gradient of the new function to back-propagate.
* This gives a single loop, which we repeat to evolve the system.
* Purge after looping for enough time.

## Using the identity

* The gradient of the __identity__ does not depend on any form of _a priori_ support.
* Hence the gradient of a persistence term, i.e., identity multiplied by a weigh, attributes weight to everything in the final distribution.
* **Note:** This lets us avoid the above complications.  
