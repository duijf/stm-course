# STM exercises

Some warm-up questions, to check your understanding:

 - What guarantees does STM provide us? What classes of bugs are eliminated by
   using it?
 - What guarantees aren't provided by STM? What do we still need to worry
   about?
 - Do you agree with the "GC for your concurrency" characterization? What are
   some arguments for and what are some arguments against it?
 - How does the Haskell type system help the programmer reason about STM
   transactions? Can you think of some invariants that need to be preserved?

## 0. Setup

If you don't have Haskell yet, install `stack` (https://haskellstack.org). Ask
me for help if you need it.

Create a new Haskell project. Use stack if you're new to this; otherwise, use
whatever you like. Add a dependency on the `stm` and `Decimal` libraries.

## 1. Continuing our contrived example

Try to recreate the bank account example from the slides.

You'll need the `Control.Concurrent.STM` and `Data.Decimal` modules, choose
a type for `Account` (alias or newtype) and implement the `transfer`, `debit`
and `credit` functions.

Add a `main` function which creates two accounts with balances and transfers
some money between them. Afterwards, print the balances of both accounts.

## 2. Experimenting with `retry`

Try to do some transactions which require prerequisites which will never become
true. See what the difference is between these two implementations of
`transfer`:

```haskell
transfer1 :: Account -> Account -> Decimal -> STM ()
transfer1 from to amount = do
  debit from amount
  credit to amount

transfer2 :: Account -> Account -> Decimal -> STM ()
transfer2 from to amount = orElse actualTransfer (pure ())
  where
    actualTransfer = do
      debit  from amount
      credit to   amount
```

Did the result surpise you? How do you think the runtime reasons about
these kinds of cases? Can you find more info on how this works?

## 3. Are our invariants really invariant?

Create a third bank account, to serve as our "bank". Give it an insane amount
of money.

Add a dependency on the `async` and `random` packages. Import
`Control.Concurrent.Async` and try to find whatever you need from the `random`
package.

Create a 5 threads using `race_` (exercise in itself). Spawn the following
function:

```haskell
thread :: Int -> [Account] -> IO ()
thread numTransactions accounts = undefined
```

Which will:

 - Loop `numTransactions` times, so we have a termination condition.
 - For each iteration: choose two accounts attempt to transfer a random amount
   of money between them. Choose your random number intervals appropriately
   (Be sure to use the `transfer2` implementation, otherwise you aren't getting
   anywhere)

After all threads have been completed, check if the total amount of money in
the system is still the same as when we started.

Bonus:

 - What can you find out about Haskell threads? What properties do they have?
 - What runtime options are available for threads? How can you use them on
   multiple cores?

## 4. Queue the next exercise

Open the documentation for the `stm` package. You can see a bunch of datatypes,
other than `TVar`. An interesting one is `TQueue`: a queue.

Try to think how you could implement a `TQueue` based of `TVar`s. How would that
work? Try not to look at the source or the STM paper, but ask for hints if
you're stuck.

One hint is free: you'll need to store read and write ends of the queue in the
same `TVar`.

Try to put your idea into code (feel free to validate first, if we're short on
time--it's in the slides of part II). Write the definition for `TQueue`. You
will need:

```
data TQueue a = ...
```

Implement the following functions for your datatype:

```
newTQueue :: STM (TQueue a)
readTQueue :: TQueue a -> STM a
writeTQueue :: TQueue a -> a -> STM ()
```

## 5. Extending our queue

Extend your custom `TQueue` into `TBQueue` so it becomes bounded (with a
configurable number of items). Good to ask yourself: when might this be useful?

How can we provide backpressure on insertion? (Do we even need to be explicit
about this?)

Let's write the code. Try to create two variants of write:

```haskell
writeBTQueue :: TBQueue a -> a -> STM ()

data InsertResult = Success | CapacityExceeded
writeTBQueue' :: TBQueue a -> a -> STM InsertResult
```

The first one blocking, the second one providing feedback to the caller about
the result.

If you're running out of things to do:

 - Try to use the `TQueue` and `TBQueue` in a work queue setting.
 - Have a single writer and multiple consumers of work items (this can just be
   printing a number, with a timeout based on how large the number is).
 - Add whatever mechansim you find interesting. Ideas: HTTP RPC, or something
   else.

## 6. For the really fast people

How would you implement STM? At what level do we need to write code? What
knowledge would a language runtime require to support STM?

Try to come up with an implementation for STM. How would we efficiently keep
transaction logs? How can we ensure that rollbacks work correctly (this is a
bit of a trick question)? How do we minimize overhead of conflict checking
accross multiple cores?
