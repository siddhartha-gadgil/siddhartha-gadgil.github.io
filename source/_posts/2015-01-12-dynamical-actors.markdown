---
layout: post
title: "Dynamical Actors"
date: 2015-01-12 09:44:57 +0530
comments: true
categories: ProvingGround
---

## Context

* Dynamical system $f: X\to X$ depending on parameters.
* Goals to check.

## Worker

* Called with state, parameter,iterations, goals.

```scala
  case class LoopWork(x: X, p: P, loops: Int, goals: X => Boolean = List())
```

* Iterates until success or loops completed.

```scala
  case class LoopsDone(id: String, final : X)

  case class Success(id: String, final : X)
```

## Database

* commits
* solutions: commit, goal.
* goals

### Commits

* State
* Hash-tag : string representing hex code of hash-tag
* Ancestor
* Meta-data: who committed, why.

```scala
  def commit(x: X, parent: String) : String // returns hash-tag
```

## Hub

* Maintains all workers and communication with outside. Messages (and a few similar ones).

```scala
  case object ActorList // returns actor ids and whether they are paused.

  case class QueryState(id: String, pause: Boolean = true)

  case class QueryParams(id: String) // look up parameters at hub.

  case class UpdateParams(id: String, p: P)

  case class UpdateState(id: String, x: X)

  case class PauseWorker(id: String)

  case class ResumeWorker(id: String)

  case class StopWorker(id: String)

  case class UpdateGoals(id: String, goals: List[X => Boolean])

  case class ActorLoops(id: String, loops: Int)
```

* For simplicity, assume that pausing on success is independent of actor, and pausing on query is part of the query string.

```scala

  case class ActorState(x: X, p: P,
    loops: Int,
    paused : Boolean,
    lastCommit: Long
    )
```

### Callback on success

* Commit the state.
* Record that the commit contains a success.
* Possibly update goals removing those attained.
* SSE log the goals.

## Channels

* from interface: post JSON to server.
* to interface: send SSE with _type_ used for a switch to decide action.
