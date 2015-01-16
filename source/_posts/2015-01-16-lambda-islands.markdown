---
layout: post
title: "Lambda Islands"
date: 2015-01-16 15:51:20 +0530
comments: true
categories:
---

## A single lambda island

* We are given a type $T$.
* Generate a new variable $x : T$. The result and gradient do not depend on $x$ (upto equality).
* We have an inclusion map $i_x: FD(V) \to FD(V)$. This is almost the identity, except the gradient purges the weight of $x$.
* From this we get the initializer on distributions, introducing $x$ with weight $p(m)$ with $m$ the $\lambda$-parameter and scaling the rest by $1 - p(m)$.
* It is probably easiest to directly define the initializer, defining its application and gradient.
* We compose the initializer with a given dynamical system.
* Finally, export by mapping $y$ to $x \mapsto y$. This is composed with the initializer and the given dynamical system.

## Combining lambda islands.

* Given an initial distribution, we have weights $p(T)$ associated to each type $T$.
* Given a dynamical system $f$ and type $T$, we have a new system $\Lambda_T(f)$.
* We take the weighted sum of these, i.e., $\Lambda(f) = \sum_T w(T) \Lambda_t(f)$.

## Using recurrence

* Given a dynamical system, generate a family $g_k$ indexed by natural numbers $k$ corresponding to iterating $2^k$ times.
* Mix in lambda to get a new family $f_k$ with $f_k = g_k + \Lambda(f_{k-1})$, with the last term a priori vanishing for $k \leq 0$.
