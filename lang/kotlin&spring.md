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

## spring webFlux

HandlerMethodArgumentResolver 이용해서 시큐리티 역할하는 유저 하고 일단 개발 진행하기도 함

Reactive Streams Specification
[Reactive Streams](https://medium.com/reactive-programming-in-springboot-using-webflux/reactive-stream-specifications-b36832a55843)

리액티브 스트림을 표준 사양을 채택한 대표적인 구현체들
- Project Reactor
- RxJava
- JDK9 Flow
- Akka Streams
- Vert.x
- Hibernate Reactive(Vert.x 사용)

FluxExtension

R2DBC 리액티브 기반의 비동기-논블로킹 데이터베이스 드라이버

Spring Data R2DBC 있

### coroutine
Mono -> suspend
Flux -> Flow

ReactiveCrudRepository를 awaitSingle 이용해서 코루틴으로 바꿀 수 있음
```kotlin
suspend fun findByUserId(userId: Long) : Content {
    return repository.findByUserId(userId).awaitSingle()
}
```
-> 또한 CoroutineCrudRepository를 바로 사용할 수 있음
여러개는 Flow<Content>

```kotlin
val job2 = launch(start = CoroutineStart.LAZY) {
    println("async task")
} 

job2.start()
```

- async {}, await() 사용하면 결과를 받을 수 있음
- suspend 일시중단이 가능하고 재개가 가능함
- coroutineScope 사용하면 블로킹 되지 않고 동작함
- flow -> collect 호출하면 다 동작


