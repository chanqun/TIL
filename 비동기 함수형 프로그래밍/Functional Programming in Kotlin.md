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

