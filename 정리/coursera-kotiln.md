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