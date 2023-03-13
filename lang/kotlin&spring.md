# kotlin & spring

## kotlin

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

Response data invoke 사용해서 변경하기도 함

## spring

HandlerMethodArgumentResolver 이용해서 시큐리티 역할하는 유저 하고 일단 개발 진행하기도 함

