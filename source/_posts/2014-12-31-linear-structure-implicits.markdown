---
layout: post
title: "Linear structure, Differentiable functions (implicit) Combinators"
date: 2014-12-31 08:52:22 +0530
comments: true
categories:
---

The best way to build differentiable functions for the typical learning system in function-finder is using _combinators_. At present there is some ad hoc version of this. The better way is to use linear structures systematically.

## Linear structures

``` scala
case class LinearStructure[A](sum: (A, A) => A, mult : (Double, A) => A){
  def diff(frm: A, remove: A) = sum(frm, mult(-1.0, remove))
}
```

Linear Spaces can be built with

* Real numbers - Double
* Finite Distributions have ++ and *
* Pairs of linear spaces
* Differentiable functions between linear spaces
* (Eventually) Maps with values in Linear spaces - for representation learning.

``` scala
case class LinearStructure[A](sum: (A, A) => A, mult : (Double, A) => A){
  def diff(frm: A, remove: A) = sum(frm, mult(-1.0, remove))
}

implicit val RealsAsLinearStructure = LinearStructure[Double]((_+_), (_*_))

implicit def VectorPairs[A, B](implicit lsa: LinearStructure[A], lsb: LinearStructure[B]): LinearStructure[(A, B)] = {
  def sumpair(fst: (A, B), scnd: (A, B)) =(lsa.sum(fst._1, scnd._1), lsb.sum(fst._2, scnd._2))

  def multpair(sc: Double, vect: (A, B)) = (lsa.mult(sc, vect._1), lsb.mult(sc, vect._2))

  LinearStructure(sumpair, multpair)
}

implicit def FiniteDistVec[T] = LinearStructure[FiniteDistribution[T]](_++_, (w, d) => d * w)
```

###To Do:

* Create a linear structure for differentiable functions
* Have functions that implicitly use linear structures.

## More vector structures

In addition, we need _inner products_ for computing certain gradients as well as _totals_ for normalization. These are built for

* Finite distributions
* pairs
* Real numbers
* (Eventually) Maps with co-domain having inner products and totals.

## Differentiable functions

``` scala
trait DiffbleFunction[A, B] extends (A => B){self =>
  def grad(a: A) : B => A

  /**
  * Composition f *: g is f(g(_))
  */
  def *:[C](that: DiffbleFunction[B, C]) = andThen(that)

  def andThen[C](that: => DiffbleFunction[B, C]): DiffbleFunction[A, C] = DiffbleFunction((a: A) => that(this(a)))(
    (a: A) =>
    (c: C) =>
    grad(a)(that.grad(this(a))(c)))
  }



  object DiffbleFunction{
    def apply[A, B](f: => A => B)(grd: => A => (B => A)) = new DiffbleFunction[A, B]{
      def apply(a: A) = f(a)

      def grad(a: A) = grd(a)
    }

  }
```

Differentiable functions are built from

* Composing and iterating differentiable functions.
* (Partial) Moves on X giving FD(X) -> FD(X)
* (Partial) Combinations on X giving FD(X) -> FD(X)
* Co-ordinate functions on FD(X).
* Inclusion of a basis vector.
* Projections and Inclusions for pairs.
* Scalar product of differentiable functions, $x\mapsto f(x) * g(x)$ with $f: V \to \R$ and $g: V\to W$.
* **Islands:** Recursive definitions in terms of the function itself - more precisely a sequence of functions at different _depths_.

### To Do:

* Clean up the present ad hoc constructors replacing them by combinators.

## Islands

* After adding an island, we get a family of differentiable functions $f_d$ indexed by depth.

* Determined by:

  * An initial family of differentiable functions $g_d$
  *  A transformation on differentiable functions $f \mapsto L(f)$.
  * An a priori bound on depth beyond which we get zero.


* Recurrence relation:

  * $f\_d = g\_d + L(f\_{d+1})$.
  * this simplifies a priori if $d$ is large enough.
