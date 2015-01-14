---
layout: post
title: "feedbacks"
date: 2015-01-09 12:03:51 +0530
comments: true
categories:
---

## Feedbacks

* A feedback is a function $f: X \to X$, typically depending on parameters, but which is _not_ differentiable (i.e., no gradient).

## Combinators

* _Linear Structure:_ We can define a linear structure on _A => B_ given one on _B_. This does not collide with that on differentiable functions as linear structures are not covariant.
* _Conjugation_: Given a differentiable function $f: X \to Y$ and a (feedback) function $g: Y\to Y$, we can define a conjugate function $g^f: X\to X$ by

$$g^f(x) = \Delta f(x)(g(f(x))).$$

* This behaves well with respect to composition of differentiable functions and linear combinations of feedbacks.
* _Partial conjugation:_ Sometimes the feedback also depends on $x \in X$, so we have $g: X\to Y$. We are then back-propagating $g$ using $f$.

## Matching distributions:

* Given a target distribution on $Y$, we get a feedback towards the target.
* Given a distribution on $X$ and $f: X\to Y$, we have a pushforward distribution. We get an image feedback.
* To get a feedback on $X$, we need to distribute among all terms with a given image proportional to the term, or depending only on the coefficient of the image image.
* **Question** Can we define this shift on $X$ in one step correctly?
* **Gradient view:** Just view $f$ as a differentiable function and back-propagate by the gradient. This shifts weights independently.

## Terms matching types

* We compare the distribution of terms that are types with the map from terms to types.
* The difference between these gives a flow on types.
* We back-propagate to get a flow on terms.

## Approximate matches

* We can filter by having a specific type, and then an error matching having a refined type (after some function application).
* For a given term, we get the product of its weight with an error factor.
* In terms of combinators, we replace inclusion by a weighted inclusion, or a product of the inclusion with a _conformal factor_. We need a new basic differentiable function.

## Blending feedbacks.

* As in the case of matching types, suppose
  * $X$ is a finite distribution.
  * various feedbacks are based on atoms.
  * we have weights for the various feedbacks.
* We give more credit for feedback components attained by fewer terms.
* This is achieved by sharing the feedback coming from one component, instead of just giving gradient
* The _terms representing types_ feedback is a special instance of this.
