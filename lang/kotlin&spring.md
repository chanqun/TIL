# kotlin & spring

### use
try with resource -> use

### 고급 예외처리
- runCatching
- recover, recoverCatching

### 람다로 프로그래밍
```kotlin
val printHello : () -> Unit = { println("hello") }

val func = printlnHello
func() // 하면 호출
```

이것도 가능
```kotlin
fun call(block: () -> Unit) {
    block()
}
```

고차 함수는 다른 함수를 인자로 받거나 함수를 반환하는 함수다.
