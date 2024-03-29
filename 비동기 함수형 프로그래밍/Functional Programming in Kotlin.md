# Functional Programming in Kotlin

## Part 1 Introduction to functional programming
### 1 What is functional programming?

Because of their modularity, pure functions are easier to test, reuse, parallelize, generalize, and reason about. 
Furthermore, pure functions are much less prone to bugs.

#### 1.1 The benefits of FP : A simple example

```kotlin
class Cafe {
    fun buyCoffee(cc: CreditCard, p: Payments): Coffee {
        val cup = Coffee()
        p.charge(cc, cup.price)
        return cup
} }
```
Although side effects still occur when we call p.charge(cc, cup.price), we have at least regained some testability.

#### 1.2 A functional solution: Removing Side effects


```kotlin
class Cafe {
    fun buyCoffee(cc: CreditCard): Pair<Coffee, Charge> {
        val cup = Coffee()
        return Pair(cup, Charge(cc, cup.price))
    }
}
```
We’ve separated the concern of creating a charge from the processing or interpretation of that charge.


```kotlin
class Cafe {
    fun buyCoffee(cc: CreditCard): Pair<Coffee, Charge> = TODO()

    fun buyCoffees(
        cc: CreditCard,
        n: Int
    ): Pair<List<Coffee>, Charge> {
        val purchases: List<Pair<Coffee, Charge>> =
            List(n) { buyCoffee(cc) }
        val (coffees, charges) = purchases.unzip()
        return Pair(
            coffees,
            charges.reduce { c1, c2 -> c1.combine(c2) }
        )
    }
}
```

The example takes two parameters: a CreditCard and the Int number of coffees to be purchased. 

#### 1.2 exactly what is a (pure) function?

We can formalize this idea of pure functions using the concept of referential transparency (RT)

#### 1.3 RT, purity, and the substitution model
```kotlin
>> val r1 = "Hello, World".reversed()
res4: kotlin.String = dlroW ,olleH
>>> val r2 = "Hello, World".reversed()
res5: kotlin.String = dlroW ,olleH
```
reversed is pure function

We can conclude that StringBuilder.append is not a pure function.


By eliminating the side effect of payment pro- cessing performed on the output, we could more easily reuse the function’s logic, both for testing and for further composition (

#### 1.4 what lies ahead

- Use lazy evaluation, and work with pure functional state.
- Understand and use monoids, monads, and applicative and traversable functors.


Summary

- Functional programming results in increased code modularity.
- Modularity gained from programming in pure functions leads to improved test-
ability, code reuse, parallelization, and generalization.
- Modular functional code is easier to reason about.
- Functional programming leads us toward using only pure functions.
- A pure function can be defined as a function that has no side effects.
- A function has a side effect if it does something other than returning a result.
- A function is said to be referentially transparent if everything it does is represented
by what it returns.
- The substitution model can be used to prove the referential transparency of a
function.

#### test 하는 것

값, state, interaction

### 2 Getting started with functional programming in kotlin

#### 2.1 higher-order functions: Passing functions to functions

Variable-naming conventions
It’s a standard convention to use names like f, g, and h for parameters to a HOF. 
In functional programming, we tend to use terse variable names, even one-letter names. 
This is because HOFs are so general that they have no opinion on what the argument should actually do in the limited scope of the function body. 
All they know about the argument is its type. 
Many functional programmers feel that short names make code easier to read since they make the code structure easier to see at a glance.

```kotlin
fun fib(i: Int): Int {
    fun go(n: Int): Int =
        if (n == 1 || n == 2) {
            1
        } else {
            go(n - 1) + go(n - 2)
        }
    return go(i)
}

fun fib(i: Int): Int {
    tailrec fun go(n: Int, now: Int, next: Int): Int =
        if (n <= 0) {
            now
        } else {
            go(n - 1, next, now + next)
        }
    return go(i, 0 ,1)
}
```
tailrec 조건 함수에 끝이 상수이거나
재귀를 할 경우 마지막이 자기 자신을 호출하는 딱 하나여야한다
꼬리 재귀는 어떤 함수가 직간접적으로 자기 자신을 호출하면서도 그 호출이 마지막 연산(tail call)인 경우

ThreadStackSize = 512인데 계속 재귀호출하면 기록이 남아있음


#### 2.2 Polymorphic functions: Abstracting over types
Often, and especially when writing HOFs, we want to write code that works for any type it’s given. These are called polymorphic functions.

##### 2.2.1 An example of a polymorphic function
```kotlin
fun <A> isSorted(aa: List<A>, order: (A, A) -> Boolean): Boolean {
    tailrec fun go(head: A, remain: List<A>): Boolean = if (remain.isEmpty()) {
        true
    } else if (!order(head, remain.head)) {
        false
    } else {
        go(remain.head, remain.tail)
    }

    return aa.isEmpty() || go(aa.head, aa.tail)
}
```
함수형에서는 head, tail이 매우 효율적

lexical scope


##### 2.2.2 Calling HOFs with anonymous functions


#### 2.3 Following types to implementations
```kotlin
fun <A, B, C> curry(f: (A, B) -> C): (A) -> (B) -> C =
        { a: A -> { b: B -> f(a, b) } }

fun <A, B, C> uncurry(f: (A) -> (B) -> C): (A, B) -> C =
    { a: A, b: B -> f(a)(b) }

fun <A, B, C> compose(f: (B) -> C, g: (A) -> B): (A) -> C =
    { a: A -> f(g(a)) }
```

자바에도 있음

순서차이
```java
default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
    Objects.requireNonNull(before);
    return (V v) -> apply(before.apply(v));
}

default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
    Objects.requireNonNull(after);
    return (T t) -> after.apply(apply(t));
}
```
ㅡㅡㅡㅡㅡㅡ

## Part 3 Functional data structures

### 3.1 Defining functional data structures
```kotlin
sealed class List<out A> // <1>

object Nil : List<Nothing>() // <2>

data class Cons<out A>(val head: A, val tail: List<A>) : List<A>() // <3>
```

### 3.2 Working with functional data structures
```kotlin
sealed class List<out A> { // <1>
    companion object { // <2>
        fun <A> of(vararg aa: A): List<A> { // <3>
            val tail = aa.sliceArray(1 until aa.size)
            return if (aa.isEmpty()) Nil else Cons(aa[0], of(*tail))
        }
        
        fun sum(ints: List<Int>): Int =
            when (ints) {
                is Nil -> 0
                is Cons -> ints.head + sum(ints.tail)
            }
        
        fun product(doubles: List<Double>): Double = 
            when(double) {
                is Nil -> 1.0
                is Cons ->
                    if (doubles.head == 0.0) 0.0
                    else doubles.head * product(doubles.tail)
            }
    }
}
```

### 3.3 Data Sharing in functional data structures

When we add an element 1 to the front of an existing list—say, xs—we return a new list, in this case Cons(1,xs). 
Since lists are immutable, we don’t need to actually copy xs; we can just reuse it.

> When mutable data is passed through a chain of loosely coupled components, 
> each component has to make its own copy of the data because other components might modify it. 
> Immutable data is always safe to share,

### 3.4 Recursion over lists and generalizing to HOFs


```kotlin
fun <A, B> foldRight(xs: List<A>, z: B, f: (A, B) -> B): B =
    when (xs) {
        is Nil -> z
        is Cons -> f(xs.head, foldRight(xs.tail, z, f))
    }
fun sum2(ints: List<Int>): Int =
    foldRight(ints, 0, { a, b -> a + b })
fun product2(dbs: List<Double>): Double =
    foldRight(dbs, 1.0, { a, b -> a * b })
```

```kotlin
Cons(1, Cons(2, Nil))
f (1,f (2,z ))
```

Sequence를 이용하면 자연수 집합을 나타낼 수 있음

### 3.5 Trees
```kotlin
sealed class Tree<out A>
data class Leaf<A>(val value: A) : Tree<A>()
data class Branch<A>(val left: Tree<A>, val right: Tree<A>) : Tree<A>()
```

- Immutable data structures are objects that can be acted on by pure functions.
- A sealed class has a finite amount of implementations, restricting the data struc-
ture grammar.
- The when construct can match typed data structures and select an appropriate
outcome evaluation.
- Kotlin matching helps work with data structures but falls short of pattern match- ing supported by other functional languages.
- Data sharing through the use of immutable data structures allows safe access without the need for copying structure contents.
- List operations are expressed through recursive, generalized HOFs, promoting code reuse and modularity.
- Kotlin standard library lists are read-only, not immutable, allowing data corrup- tion to occur when acted on by pure functions.
- Algebraic data types (ADTs) are the formal name of immutable data structures and can be modeled by data classes, Pairs, and Triples in Kotlin.
- Both List and Tree developed in this chapter are examples of ADTs.


## Part 4 Handling error without exceptions

- Pitfalls of throwing exceptions
- Understanding why exceptions break referential transparency
- Handling exceptional cases: a functional approach
- Using Option to encode success and ignore failure
- Applying Either to encode successes and failures

### 4.1 The problems with throwing exceptions
- In functional programming, we avoid throwing exceptions except under extreme circumstances where we cannot recover.
- Exceptions are not type-safe. If we forget to check for an exception in failingFn, a thrown exception won’t be detected until run time.

### 4.2 Problematic alternatives to exceptions

- Sentinel value 보초 값
- Supplied default value

### 4.3 Encoding success conditions with Option

```kotlin
sealed class Option<out A>
data class Some<out A>(val get: A) : Option<A>()
object None : Option<Nothing>()

fun mean(xs: List<Double>): Option<Double> =
    if (xs.isEmpty()) None
    else Some(xs.sum() / xs.size())
```

### 4.4 Encoding success and failure conditions with Either

```kotlin
sealed class Either<out E, out A>
data class Left<out E>(val value: E) : Either<E, Nothing>()
data class Right<out A>(val value: A) : Either<Nothing, A>()
```


- Throwing exceptions is not desirable and breaks referential transparency.
- Throwing exceptions should be reserved for extreme situations where recovery
is not possible.
- You can achieve purely functional error handling by using data types that
encapsulate exceptional cases.
- The Option data type is convenient for encoding a simple success condition as
Some or a failure as an empty None.
- The Either data type can encode a success condition as Right or a failure condition as Left.
- Functions prone to throwing exceptions may be lifted to be compliant with
Option and Either types.
- A series of Option or Either operations may be halted on the first failure
encountered.
- The for-comprehension is a construct that allows the fluid expression of a series of
combinator calls.
- The Arrow library, a functional companion to Kotlin, has the Either construct
that allows for-comprehensions through binding methods to simplify code.

4장 스터디


js promise (flatmap 계속하는 것 같다), kotlin coroutine, 
함수형 형식을 유지해주는 것이 webFlux Mono 또는 Flux chain 안에서 처리가 된다.

함수형 스타일로 비동기 처리한다.

## Part 5 Strictness and laziness

The notion of non-strict or lazy evaluation comes to the rescue here the value is only computed at the point where it is actually referenced, not where it is declared. 
We are no longer greedy in performing all the calculations but instead compute them on demand.

### 5.1 Strict and non-strict functions
&&, ||, if is non-strictness

### 5.2 An extended example: Lazy lists
Stream 

### 5.2.1 Memoizing streams and avoiding recomputation

### 5.3 Separating program description from evaluation
The stream elements themselves could be large objects that retain significant amounts of memory. 
Reclaiming this memory as quickly as possible can cut down on the amount of memory required by our program as a whole.


### 5.4 Producing infinite data streams through corecursive functions
Whereas a recursive function consumes data, a corecursive function produces data. 
And whereas recursive functions terminate by recursing on smaller inputs, corecursive functions need not terminate as long as they remain productive, which just means we can always evaluate more of the result in a finite amount of time.


### Summary

- Strict expressions are evaluated at the moment they are bound to a variable. This is acceptable for simple expressions but not for expensive computations, which should be deferred as long as possible.
- Non-strict or lazy evaluation results in computations being deferred to the point where the value is first referenced. This allows expensive computations to be evaluated on demand.
- A thunk is the unevaluated form of an expression and is a humorous past participle of "think."
- Lazy initialization can be achieved by wrapping an expression in a thunk, which can be forced to execute explicitly at a later stage if required.
- You can use the Stream data type to model a lazy list implementation using Cons and Empty sealed types.
- Memoizing is a technique used to prevent multiple evaluations of an expression by caching the first evaluation result.
- A smart constructor provides a function with a slightly different signature than the actual constructor. It ensures some invariants in addition to what the original offers.
- We can separate the concerns of description and evaluation when applying laziness, resulting in the ability to describe a larger expression than needed while only evaluating a smaller portion.
- Infinite streams can be generated by using corecursive functions to produce data incrementally. The unfold function is such a stream generator.

foldRight는 list를 Cons로 바꾸는 것이라고 생각하자

zipWith, zipAll - 두 스트림의 길이 차이
끝나면 끝, 끝나도 긴 것으로 맞춘다 

## Part 6 Purely functional state

By viewing the program state as a transition or action that is passed along as context during a series of transformations, 
we can contain and localize the complexity associated with state machines.
We can even take this a step further by hiding these state transitions altogether through the use of higher-order combinator functions that pass along state actions implicitly in the background.


### 6.1 Generating random numbers using side effects

Kotlin random
These methods aren’t referentially transparent because the state updates are performed as a side effect.
this implies that they aren’t as testable, composable, modular, and easily parallelized as they could be.

### 6.2 Purely functional random number generation

### 6.3 Making stateful APIs pure

### 6.4 An implicit approach to passing state actions
#### 6.4.1 More power by combining state actions
We would like to keep following this approach when implementing intDouble and doubleInt from exercise 6.3, but map simply can’t do this. We need a new combinator, map2, that can combine two RNG actions into one using a binary rather than a unary function.

#### 6.4.2 Recursive retries through nested state actions

#### 6.4.3 Applying the combinator API to the initial example

### 6.5 A general state action data type

### 6.6 Purely functional imperative programming

- Updating state explicitly allows the recovery of referential transparency, which is lost in situations when mutating state.
- Stateful APIs are made pure by computing each subsequent state, not mutating the existing state.
- A function that computes a state based on a previous state is known as a state action or state transition.
- You can use combinators to abstract repetitive state transition patterns and even combine and nest state actions when required.
- You can adopt an imperative style when dealing with state transitions through the use of for-comprehensions.
- Arrow offers a proper State API and associated data type to model all the con- cepts dealt with in this chapter.


## Part 7 Purely functional parallelism

동시성을 만드는 것은 side effect다.
순수 함수형 프로그래밍 처럼 만드는 것이 7장의 묘미다.

### 7.1 Choosing data types and functions
#### 7.1.1 A data type for parallel computations

```kotlin
class Par<A>(val get: A)
fun <A> unit(a: () -> A): Par<A> = Par(a())
fun <A> get(a: Par<A>): A = a.get
```

#### 7.1.2 Combining parallel computations to ensure concurrency
Let’s see if we can avoid the pitfall mentioned earlier of combining unit and get. 

#### 7.1.3 Making computations to be forked explicity

### 7.2 Picking a representation

### 7.3 Refining the API with the end user in mind

### 7.4 Reasoning about the API in terms of algebraic equations
#### The law of mapping
#### The law of forking
#### Using actors for a non-blocking implementation


### 7.5 Refining combinators to their most general form



- Functional API design is an iterative and exploratory process driven by real world examples.
- A purely functional library that deals with parallelization is a perfect example to demonstrate API design.
- Data types and their associated functions are born out of exploring domain examples.
- Treating an API as you would an algebraic equation leads to following the types to a concrete implementation.
- Laws help define constraints on operations, lead to implementation choices, and validate properties.
- Combinators can often be generalized to broaden their application across many different applications and scenarios.
- Effective library design separates the description of computations from the interpreter responsible for running them.

스터디 종료
