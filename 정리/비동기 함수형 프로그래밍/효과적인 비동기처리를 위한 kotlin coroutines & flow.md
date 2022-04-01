[효과적인 비동기처리를 위한 kotlin coroutines & flow 마스터](https://fastcampus.co.kr/courses/209503/clips/)

### 동시성

동시성 없이는 사용자가 불편하, 하드웨어 리소스도 낭비 된다

비선점형 - 블루 스크린,

선점형

- 프로세스
    - 운영체제는 1개 잇아의 프로세스를 시분할에 의해 번갈아서 동작을 시킴 코드, 데이터, 힙, 스택등의 자료구조를 가짐
- 스레드
    - 프로세스는 1개 이상의 스레드를 가지며 시분할에 의해 번갈아서 동작 일반적으로 스택을 구분하고 나머지는 공유하지만 운영체제 마다 다름
    - 리눅스는 copy on write라 프로세스와 스레드 차이가 크지 않고 윈도에서는 프로세스의 생성이 느리기 떄문에 스레드의 중요성이 커짐

### 병렬성

- 여러 프로레스가 하나의 메모리를 쓰는 모델
- 여러 프로세스가 물리적으로 여러 일을 동시에 사용하는 일이 가능해짐

#### SMP (Symmetric Multiprocessor)와 가시성

- 여러 cpu에서 개별 캐쉬를 사용하게 되면서 발생한 문제
-
- 락이나 메모리 베리어가 필요할 수 있음.

> 비동기 병렬 프로그래밍에는 함정이 많다!!
콜백과 rxJava가 어느 정도 해결했다

#### 코루틴 빌더

```kotlin
fun main() = runBlocking {
    println(Thread.currentThread().name)
    println("Hello")
}
```

BlockingCoroutine은 CoroutineScope의 자식

#### 코루틴 컨텍스트

코루틴 스코프는 코루틴을 제대로 처리하기 위한 정보

```kotlin
println(coroutineContext)

launch {
}

delay(500L)
```

```kotlin
fun main() {
    runBlocking {
        launch {
            println("launch1: ${Thread.currentTread() name}")
            delay(1000L)
            println("3")
        }
        launch {
            println("launch2: ${Thread.currentTread() name}")
            delay(2000L)
            println("1")
        }
        println("runBlocking: ${Thread.currentTread() name}")
        delay(500L)
        println("2")
    }
    println("4")
}
```

suspend - 중단 가능한 함수 다른 코루틴이나 suspend 함수를 호출 할 수 있다.

## 구조화된 동시성

launch는 반드시 coroutine 안에서 수행해야함

밖에서 coroutine block이 있어야함

```kotlin
suspend fun doOneTwoThree() = coroutineScope { // this: 코루틴 : 자식 코루틴이 다 끝날때까지 기다린다.
    val job = launch { // this: 코루틴  receiver 수신객체이다
        // 자식으로서 새로운 코루틴이 만들어진다.
    }

    job.join() // suspension point

    launch {

    }

    launch {

    }
}

fun main() = runBlocking {
    doOneTwoThree()
}

```

코루틴은 협력적으로 동작하기 대문에 여러 코루틴을 만드는 것이 큰 비용이 들지 않는다. 10만개의 간단한 일을 하는 코루틴도 큰 부담이 아니다.

### 취소와 타임아웃

명시적인 job에 대해 취소를 할 수 있음

```kotlin
suspend fun doOneTwoThree() = coroutineScope {
    val job = launch {
    }

    job.cancel()
}
```

취소 불가능한 job

launch(Dispatchers.Default)는 그 다음 코드 블록을 다른 스레드에서 수행을 시킨다.

```kotlin
suspend fun doCount() = coroutineScope {
    val job1 = launch(Dispatchers.Default) {
        var i = 1

        while (i <= 10 && isActive) {
            i++
            println("count $i")
        }
    }

//    job1.cancel()
//    job1.join()

    job1.cancelAndJoin()

    println("doCount done!")
}
```

cancel + join -> cancelAndJoin


















