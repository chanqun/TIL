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

