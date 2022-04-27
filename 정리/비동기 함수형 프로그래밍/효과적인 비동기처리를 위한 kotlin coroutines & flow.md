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

### finally 를 같이 사용

suspend 함수들은 JobCancellationException 를 발생하기 때문에 표준 try catch finally로 대응할 수 있다.

#### 취소 불가능한 코드

```kotlin
withContext(NonCancellable) {
}
```

#### 타임 아웃

일정 시간이 끝난 후에 종료하고 싶으면

```kotlin
withTimeout(500L)
```

#### withTimeoutOrNull

작업이 성공하면 끝 실패하면 null을 리턴한다.

#### 서스펜딩 함수

async 키워드를 이용하면 동시에 다른 블록을 수행할 수 있다. launch와 비슷하게 보이지만 수행 결과를 await키워드를 통해 받을 수 있다는 차이가 있다.

결과를 받아야 한다면 async 결과를 받을 필요 없으면 launch 결과를 받으려면 await 을 사용하기만 하면 알아서 처리된다.

늦게 시작하는 이런 것도 있다.

```kotlin
val value1 = async(start = CoroutineStart.LAZY) { getRandom1() }

value1.start() // 큐에 실행 예약을 한다.
```

### 예외처리

코드를 수행하다 보면 예외가 발생할 수 있다. 예외가 발생하면 위쪽의 코루틴 스코프와 아래쪽의 코루틴 스코프가 취소됩니다.

다른 형제가 문제가 생기면 나와 부모가 cancel 된다.

### 코루틴 컨텍스트와 디스패치

```kotlin
fun main() = runBlocking<Unit> {
    launch {
        println(Thread.currentThread().name)
    }

    launch(Dispatchers.Default) {
        println(Thread.currentThread().name)
    }

    launch(Dispatchers.IO) {
        println(Thread.currentThread().name)
    }

    launch(Dispatchers.Unconfined) {
        println(Thread.currentThread().name)
    }

    launch(newSingleThreadContext("Coroutine")) {
        println(Thread.currentThread().name)
    }
}
```

1. Default는 코어 수에 비례하는 스레드 풀에서 수행
2. IO는 코어 수 보다 훨씬 많은 스레드를 가지는 스레드 풀이다. IO 작업은 CPU를 덜 소모
3. Unconfined는 어디에도 속하지 않는다.
4. newSingleThreadContext는 항상 새로운 스레드를 만든다.

async await withContext 에서도 사용가능하다.

Confined는 처음에는 부모의 스레드에서 수행된다. 한 번 중단점에 오면 바뀌게 된다.



코루틴 스코프, 코루틴 컨텍스트는 구조화되어 있고 부모에게 계층적으로 되어 있다. 코루틴 컨텍스트의 Job역시 부모에게 의존적

```kotlin
val job = launch(Job()) { // 부모 자식 관계가 끊기게 됨
}
```

여러 코루틴 엘리먼트를 한 번에 사용할 수 있다.

직관적으로 사용할 수 있음
```kotlin
launch {
    launch(Dispatchers.IO + CoroutineName("launch1")) { // 부모의 코루틴 컨텍스트 + CoroutineContext  
    }
}
```


### CEH와 슈퍼바이저 잡

#### GlobalScope

GlobalScope는 어떤 계층에도 속하지 않고 영원히 동작하게 된다는 문제점이 있다.

#### CoroutineScope
인자로 CoroutineContext를 받는데 코루틴 엘리먼트를 하나만 넣어도 좋고 다른 것을 넣어서 조합해도 좋다.


#### CHE (Coroutine Exception Handler)

```kotlin
val ceh = CoroutineExceptionHandler { _, exception ->
    println("Something happen : $exception")    
}

fun main() = runBlocking<Unit> {
    val scope = CoroutineScope(Dispatchers.IO)
    val job = scope.launch(ceh) {
        launch { printRandom1() }
        launch { printRandom2() }
    }
    job.join
}
```

runBlocking에서는 CEH를 사용할 수 없다.

SupervisorJob() 에러가 발생하면 아래쪽으로만 가도록 할 수 있다.


#### SupervisorScope

```kotlin
suspend fun supervisordFunc() = supervisorScope {
    launch {}
    launch(ceh) {} // supervisorScope 내에서 처리하지 않으면 위로 전파 된다.
}
```

### 뮤텍스

상호배제의 줄임말 (Mutex exclusion)
- 공유 상태를 수정할 때 임계 영역을 이용하게 되며, 임계 영역을 동시에 접근하는 것을 허용하지 않는다.

```kotlin
val mutex = Mutex()

fun main() = runBlocking {
    withContext(Dispatchers.Default) {
        massiveRun {
            mutex.withLock {
                couter++
            }
        }
    }
}
```

### 액터

액터가 독점적으로 자려를 가지며 다른 코루틴과 공유하지 않고 액터를 통해서만 접근하게 만든다.

```kotlin
sealed class CounterMsg
object IncCounter : CounterMsg()
class GetCounter(val response: CompletableDeffered<Int>) : CouterMsg()


fun CoroutineScope.counterActor() = actor<CouterMsg> {
    var counter = 0
  
    for (msg in channel) {
        when (msg) {
            is IncCounter -> counter++
            is GetCounter -> msg.response.complete(counter)
        }
    }
}

fun main() = runBlocking<Unit> {
    val counter = counterActor()
    
    withContext(Dispatchers.Default) {
        massiveRun {
            counter.send(IncCounter)
        }
    }
  
    val response = CompletableDeffered<Int>()
    counter.send(GetCounter(response))
    println("Counter = ${response.await()}")
    counter.close()
}
```

채널은 송신 측에서 값을 send 할 수 있 수신 측에서 receive를 할 수 있는 도구이다.

## 비동기 데이터 스트림

### 플로우 기초

Flow는 코틀린에서 쓸 수 있는 비동기 스트림

```kotlin
fun flowSomething(): Flow<Int> = flow {
    repeat(10) {
        emit(Random.nextInt(0, 500))
        delay(10L)
    }
}

fun main() = runBlocking {
    flowSomething().collect { value ->
        println(value)
    }
}
```
콜드 스트림임
요청하면 주기 시작함

#### 플로우 취소
````kotlin
withTimeoutOrNull(500L) {}
````
#### flowOf, asFlow
````kotlin
flowOf(1, 2, 3)

(1..3).asFlow()
````

### 플로우 연산

map, filter, filterNot 있음

transForm
```kotlin
suspend fun someCalc(i: Int): Int {
    return i * 2
}

fun main() = runBlocking<Unit> {
    (1..20).asFlow().transform {
        emit(it)
        emit(someCalc(it))    
    }.collect {
        println(it)
    }
    
}
```

### takeWhile
````kotlin
.takeWhile {
  it < 15
}
````
조건을 만족하는 동안만 가져온다 

### drop, dropWhile

### reduce 연산자
첫번째 값을 결과에 넣은 각 값을 가져와 누진적으로 계산하는 것

### fold
fold 연산자는 초기값이 있는 reduce와 같다

### count 연산자

count 종단 연산자, terminal operator 특정 값, 컬렉션, 결과

filter 같은 것은 중간 연산자. 결과 X collect를 써야한다.

### 플로우 컨텍스트
플로우 내에서는 컨텍스트 바꿀 수 없음


```kotlin
fun simple(): Flow<Int> = flow {
    for (i in 1..10) {
        emit(i)
    } // 업스트립
}
.flowOn(Dispatcher.Default)
.map { // 다운 스트림 : 상대적인 것 flowOn 이동해서 바꿀 수 있음
    it * 2
}

```
### 플로우 버퍼링

#### buffer
buffer로 버퍼를 추가해 보내는 측이 더 이상 기다리지 않게 한다.

```kotlin
fun main() = runBlocking<Unit> {
    val time = measureTimeMills {
        simple().buffer()
          .collect { value ->
                delay(300)
                println(value)
          }
    }
}
```
보내는 쪽은 거의 기다릴 필요 없음

#### conflate

conflate를 이용하면 중간의 값을 융합할 수 있다 - 중간의 값을 버릴 수 있음

#### collectLatest
첫번째 값을 받고 다른 값을 받으면 계속 리셋된다.


### 플로우 결합하기

#### zip
zip은 양쪽의 데이터를 한꺼번에 묶어 새로운 데이터를 만들어 낸다.


combine 양쪽의 데이터를 같은 시점에 묶지 않고 한 쪽이 갱신되면 새로 묶어 데이터를 만든다.
```kotlin
fun main() = runBlocking<Unit> {
    val nums = (1..3).asFlow()
    val strs = flowOf("일", "이", "삼")
  
    nums.combine(strs) { a, b -> "{a}는 $b"}
      .collect { println(it) }
}
```
1 일
2 일
3 일

속도에 따라 다름
데이터가 짝을 이룰 필요없이 최신의 데이터를 이용해 가공해야 하는 경우 사용할 수 있음





















