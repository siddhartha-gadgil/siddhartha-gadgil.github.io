---
layout: post
title: "gradient of product"
date: 2015-01-02 10:38:10 +0530
comments: true
categories: Proving-Ground
---

A subtle differentiable function is the scalar product,

$$f: (\alpha, v) \mapsto \alpha v$$

for a vector space $V$.

## Derivative

By Liebniz rule,  the total derivative is

$$Df(\alpha, v)(\alpha', v') = \alpha v' + \alpha' v$$

## Gradient

Note that depends on the vector space structure on $(\mathbb{R}, V)$. For a vector $w$ in $V$, note that we can write

$$<Df(\alpha, v)(\alpha', v'), w> = \alpha' <v, w> + <v', \alpha w> = <(\alpha', w'), (<v, w>, \alpha w)$$

By definition of gradient (as adjoint derivative), we have

$$\Delta f (\alpha, v)(w) = (<v, w>, \alpha w)$$

## Code

We need an additional implicit structure with combinators:

```scala
case class InnerProduct[V](dot: (V, V) => Double)

def vdot[V](implicit ip: InnerProduct[V]) = ip.dot
```
