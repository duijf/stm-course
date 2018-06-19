# STM slides & exercises

This repo contains slides and exercises from a talk series I gave on Software
Transactional Memory at Monadic Party in June 2018.

## Outline

I used my five hours at Monadic Party in the following way:

  1. Transactions of Actions ([slides][slides-1], [video][video-1])
  1. Exercises 1 - 3 ([link][exercises])
  1. Transactional LEGOs ([slides][slides-2], [video][video-2])
  1. Exercises 4 - 6 ([link][exercises])
  1. Operational Observations ([slides][slides-3], [video][video-3])

The first part is intended to give an overview of the API, and talk about the
problems that STM aims to solve. The exercises help you verify that you can
actually use the API, which was presented.

The second part shows how to build your own abstractions out of the `TVar`s
that STM gives you. It also contains some notes on contention and fairness. I
show how to build `TMVar`s, `TQueue`s, and discuss some high level notes about
the datatypes in `stm-containers`.

## Resources

If you want to learn more about concurrent Haskell (implementation or how to
use it), I suggest the following reading material:

 - [Composable Memory Transactions][stmpaper] (Paper)
 - [Lightweight Concurrenty Primitives for GHC][conprimpaper] (Paper)
 - [Parallel and Concurrent Programming in Haskell][parbook] (Book)

If you want to write concurrent Haskell code, the following packages might be
useful (there are many more, but I've used these):

 - [`async`][asyncpkg]
 - [`stm`][stmpkg]
 - [`stm-containers`][stmcontpkg]

 [video-1]:https://www.youtube.com/watch?v=vG7J9YeBUK8
 [video-2]:https://www.youtube.com/watch?v=aBOOFYeTd1g
 [video-3]:https://www.youtube.com/watch?v=CNamlutuczc
 [slides-1]:https://github.com/duijf/stm-course/blob/master/part1.pdf
 [slides-2]:https://github.com/duijf/stm-course/blob/master/part2.pdf
 [slides-3]:https://github.com/duijf/stm-course/blob/master/part3.pdf
 [exercises]:https://github.com/duijf/stm-course/blob/master/exercises.md
 [parbook]:https://simonmar.github.io/pages/pcph.html
 [conprimpaper]:https://dl.acm.org/citation.cfm?id=1291217
 [stmpaper]:https://www.microsoft.com/en-us/research/wp-content/uploads/2005/01/2005-ppopp-composable.pdf
 [asyncpkg]:https://hackage.haskell.org/package/async
 [stmpkg]:https://hackage.haskell.org/package/stm
 [stmcontpkg]:https://hackage.haskell.org/package/stm-containers
