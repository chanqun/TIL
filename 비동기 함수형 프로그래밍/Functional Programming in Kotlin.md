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
```


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

