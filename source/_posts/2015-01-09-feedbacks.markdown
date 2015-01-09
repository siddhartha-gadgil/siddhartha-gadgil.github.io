---
layout: post
title: "feedbacks"
date: 2015-01-09 12:03:51 +0530
comments: true
categories:
---

### Matching distributions:

* Given a target distribution on $Y$, we get a feedback towards the target.
* Given a distribution on $X$ and $f: X\to Y$, we have a pushforward distribution. We get an image feedback.
* To get a feedback on $X$, we need to distribute among all terms with a given image proportional to the term, or depending only on the coefficient of the image image.
* **Question** Can we define this shift on $X$ in one step correctly?
* **Gradient view:** Just view $f$ as a differentiable function and back-propagate by the gradient. This shifts weights independently.

## Terms matching types

* We compare the distribution of terms that are types with the map from terms to types.
* The difference between these gives a flow on types.
