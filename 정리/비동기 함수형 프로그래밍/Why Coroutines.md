[Android Coroutine](https://developer.android.com/kotlin/first)

## Coroutine

- 코루틴은 루틴의 일정
- 협동 루틴이라 할 수 있다.

> 코루틴은 이전에 자신의 실행이 마지막으로 중단되었던 지점 다음의 장소에서 실행을 재개한다.

- A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously.
- On Android, coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app
  to become unresponsive.

- Coroutines are a Kotlin feature that converts async callbacks for long-running tasks, such as database or network
  access, into sequential code

비동기(async)를 순차적으로 처리할 수 있도록 해주는 것이다.

```kotlin
suspend fun loadUser() {
    val user = api.fetchUser()
    show(user)
}
```

callback 도 없고 thread 처리도 없었지만 쓰레드 이동 예외처리가 되어 있음

![img](../image/coroutine-learning-curves.png)

## Basics

```kotlin
// 일시 중단이 될 수 있다는 뜻
suspend fun myWorld() {
    delay(1000L)
    println("world.")
}
```

coroutine 이 thread 보다 가볍다

```kotlin
repeat(100_000) {
    println(".")
}
```

- process가 끝나면 coroutine도 끝난다
- delay도 suspend fun
- launch는 스케줄러 느낌

## Cancellation and Timeouts

```kotlin
fun main() = runBlocking {
    job = launch {
        try {
            repeat(1000) {
                delay(500L)
            }
        } finally {
            withConext(NonCacnellable) {
                delay(1000L)
            }
        }
    }

    delay(1300L)
    job.cancel() //
    job.join()
}
```

- Coroutine cancellation is cooperative
- A coroutine code has to cooperate to be cancellable
- suspending functions are cancellable

> yield() 는 delay를 안 주고도 취소할 수 있음
> cancel될 때 exception을 던짐 CancellationException

- way 1 : to periodically invoke a suspending
- way 2 : explicitly check the cancellations status (isActive)

resource 해제 블럭은 finally 에서 하면 됨

```kotlin
fun main() = runBlocking {
    val result = withTimeoutOrNull(1300L) {
        repeat(1000) {
            delay(500L)
        }
    }
    println("Result is $result")
}
```

Sequential by default

```kotlin

fun main() = runBlocking {
    val time = measureTimeMillis {
        val one = async { doSomethingUsefulOne() }
        val two = async { doSomethingUsefulTwo() }
        println("${one.await()}  ${two.await()}")
    }

    println("Completed in $time ms")
}

suspend fun doSomethingUsefulOne()

suspend fun doSomethingUsefulTwo()
```

- to simplify code that executes asynchronously.
- converts async callbacks to sequential code.
- Use suspend functions to make async code sequential.


async { }
- Note that concurrency with coroutines is always explicit.


LAZY를 걸어서 나중에 실행하게 할 수 있다.
```
val one = async(start = CoroutineStart.LAZY) { doSomething() }
one.start()
```
start or await를 호출해야 한다.

--> structured concurrency (GlobalScope XXXXX) , exception에 대한 핸들링이 떨어지게 됨

coroutine 안 에서만 사용할 수 있도록 만들어라


## Coroutines under the hood

### Continuation Passing Style (CPS)

CPS로 알아서 compile 해줌

bytecode 보면

Continuation 객체를 넘기게 바뀜 -> 작성한 함수가 내부적으로 switch 문으로 바뀌고 -> suspend 함수 호출할 때 마다 cont 만듬 (callback interface 같은 것)









