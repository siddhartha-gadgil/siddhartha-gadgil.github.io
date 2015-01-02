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

* Add a field for the zero vector (done).
* Create a linear structure for differentiable functions(done).
* Have functions that implicitly use linear structures(done).

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
* Scalar product of differentiable functions, $x\mapsto f(x) * g(x)$ with $f: V \to \mathbb{R}$ and $g: V\to W$.
* The sum of a set of differentiable functions X \to Y, with the set of functions depending on $x : X$.

From the above, we should be able to build.

* **Islands:** Recursive definitions in terms of the function itself - more precisely a sequence of functions at different _depths_ (in a more rational form).
* **Multiple islands:** A recurrence corresponding to a lot of island terms.

### To Do:

* Clean up the present ad hoc constructors replacing them by combinators.

## Islands

* After adding an island, we get a family of differentiable functions $f_d$ indexed by depth.
* This can be simply viewed as a recursive definition.
* Given a function $g$, we get a function at depth $d$ by iterating $2^d$ times by repeated squaring.

* Determined by:

  * An initial family of differentiable functions $g_d$, corresponding to iterating $2^d$ times.
  * A transformation on differentiable functions $f \mapsto L(f)$.


* Recurrence relation:

  * $f\_d = g\_d + L(f\_{d-1})$, for $d \geq 0$, with $f_{-1} = 0$.
  * Here $L(f) = i \circ f \circ j$.
  * When $d=0$, the recurrence relation just gives just gives $id = id$.
  * Then $d=1$, we get $f\_1 = g\_1 + L(id)$.


## Multiple Islands

* We have a set of islands associated to a collection of objects, with a differentiable function associated to each.
* We can assume the set to be larger, with functions for the rest being zero. So the set is really an a priori bound.
* We get a recurence relation:
* $f\_d(\cdot) = g\_d(\cdot) + \sum_v i\_v(\cdot) * L\_v(f\_{d-1}(\cdot)).$
* Here $i\_v$ is evaluation at $v$, for a pair of finite distributions.
