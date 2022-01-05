# Kotlin for Java Developers

https://www.coursera.org/learn/kotlin-for-java-developers/home/welcome

### 1. Kotlin Philosophy
- Concise
- Safe
- Interoperable
- Tool-friendly

### 2. From Java to Kotlin

automatically convert java code to kotlin code

```kotlin
data class
-> equals, hashCode, toString
```

```kotlin
fun updateWeather(degrees: Int) {
    val (description, color) = when {
        degrees < 10 -> "cold" to BLUE
        degrees < 20 -> "warm" to RED
    }
}
```

#### Hello, world example (expression) / 

#### variables (read-only list) / 

#### functions / 
```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```

function returning Unit
function everywhere

@JvmName annotation can change name

#### named & default arguments

```kotlin
fun displaySeparator(size: Int, character: Char) {
    repeat(size) {
        print(character)
    }
}
```

```kotlin
@JvmOverloads
fun sum(a: Int = 0, b: Int = 0, c: Int = 0)
```

Only 4 overloaded functions are generated



#### Control Structures
No break is needed
```kotlin
when (pet) {
    is Cat -> pet.meow()
    is Dog -> pet.woof()
}
```

withIndex()


#### exceptions
no difference between checked and unchecked exceptions

for java
```kotlin
@Throws(IOException::class)
fun bar() {
    throw IOException()
}
```

#### extension function

infix function
ex) map To, until

```kotlin
val regex = """\d{2}""".toRegex()
```

Warning: Extension is shadowed by a member