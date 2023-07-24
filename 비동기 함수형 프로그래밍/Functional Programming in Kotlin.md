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