[inflearn rxJava](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EB%A6%AC%EC%95%A1%ED%8B%B0%EB%B8%8C%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-1/lecture/41592


## reactive programming?

> In computing, reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.

Push 방식 : 데이터의 변화가 발생했을 때 변경이 발생한 곳에서 데이터를 보내주는 방식

- RTC
- 소켓 프로그래밍
- DB Trigger
- Spring의 ApplicationEvent
- Angular의 데이터 바인딩
- 스마트폰의 push 메시지

Pull 방식 : 변경된 데이터가 있는지 요청을 보내 질의하고 변경된 데이터를 가져오는 방식

- 클라이언트 요청 & 서버 응답 방식의 애플리케이션



> Observable : 데이터 소스
> 리액티브 연산자(Operators) : 데이터 소스를 처리하는 함수
> 스케줄러(Scheduler) : 스레드 관리자
> 함수형 프로그래밍 : RxJava에서 제공하는 연산자 함수를 사용



## 마블 다이어그램

리액티브 프로그래밍을 통해 발생하는 비동기적인 데이터의 흐름을 시간의 흐름에 따라 시각적으로 표시한 다이어그램
- 문장으로 적혀 있는 리액티브 연산자의 기능을 이해하기위한 핵심 도구

| -> 데이터 발행이 정상적으로 끝남
x -> 에러가 발생한 후 종료 됨

```java
filter
- 1 - 25 - 9 - 15 - |
filter(x => x > 10)
- - - 25 - - - 15 - |


public static void main(String[] args) {
    Observable.just(1, 25, 9)
        .filter(x -> x > 10)
        .subscribe(x -> System.out.println(x));
}

```

just : Observable 이나 flowable을 반환한다.


## reactive streams 란?
- 리액티브 프로그래밍 라이브러리의 표준 사양
- 인터페이스만 제공
- Publisher, Subscriber, Subscription, Processor 라는 4개의 인터페이스를 제공

Publisher: 데이터를 생성하고 통지
Subscriber: 통지된 데이터를 전달받아서 처리
Sbuscription: 전달 받은 데이터의 개수를 요청하고 구독을 해지
Processor: Publisher와 Subscriber 기능이 모두 있음


### Cold Publisher & Hot Publisher
Cold
- 생사자는 소비자가 구독 할때마다 데이터를 처음부터 새로 통지

Hot
- 생산자는 소비자 수와 상관없이 데이터를 한 번만 통지

Processor, Subject로 끝나면 hot publisher라고 볼 수 있음


```java
PublishProcessor<Integer> processor = PublishProcessor.create();

processor.subscribe(data -> System.out.println("구독자 1:" + data));
processor.onNext(1);

processor.subscribe(data -> System.out.println("구독자 1:" + data));
processor.onNext(2);

processor.onComplete();
```


### Flowable과 Observable

Flowable
- reactive Streams 인터페이스를 구현함
- 배압 기능이 있음
Observable
- reactive Streams 인터페이스를 구현하지 않음
- 배압 기능이 없음

배압(BackPressure)이란?
Flowable에서 데이터를 통지하는 속도가 Subscriber에서 통지된 데이터를 전달받아 처리하는 속도 보다 빠를 때 밸런스를 맞추기 위해 데이터 통지량을 제어하는 기능

- MISSING 전략
- ERROR 전략
- BUFFER
  - DROP_LATEST 가장 최근에 버퍼로 들어온 데이터를 DROP하고 기다리던 데이터를 채움
  - DROP_OLDEST 버퍼가 가장 오래 된 데이터를 버리고 채우게 됨
- DROP 전략 - 이후 생성되는 데이터를 버림
- LATEST 버퍼에 데이터가 모두 채워진 상태가 되면 버퍼가 비워질 떄까지 통지된 데이터는 버퍼 밖에서 대기

## Single vs Maybe vs Completable

### Single
- 데이터를 1건만 통지하거나 에러를 통지
- 데이터 통지 자체가 완료를 의미
- 데이터를 1건만 통지하므로 데이터 개수를 요청할 필요가 없다
- onSuccess() 제공
- 대표적 소비자 SingleObserver
- 클라이언트의 요청에 대응하는 서버의 응답이 Single을 사용하기 좋은 대표적인 예이다.


### Maybe
- 데이터를 1건만 통지하거나 1건도 통지하지 않고 완료 또는 에러
- 데이터 통지 자체가 완료를 의미
- 데이터를 1건도 처리하지 않고 처리가 종료될 경우에는 완료 통지를 한다.
- Maybe의 대표적인 소비자는 MaybeOvserver 이다.



Completable
- 데이터 생산자이지만 데이터를 1건도 통지하지 않고 완료 또는 에러를 통지한다.
- 데이터 통지의 역할 대신에 Completable 내에서 특정 작업을 수행한 후, 해당 처리가 끝났음을 통지하는 역할을 한다.



### 함수형 인터페이스와 람다

- 함수형 인터페이스는 말 그대로 java의 interface
- 함수형 인터페이스는 단 하나의 추상 메서드만 가지고 있는 인터페이스
- 함수형 인터페이스의 메서드를 람다 표현식으로 작성해서 다른 메서드의 파라미터로 전달할 수 있다.
- 람다 표현식 전체를 해당 함수형 인터페이스를 구현한 클래스의 인스턴스로 취급한다

@FunctionalInterface


### 람다 표현식

- 익명 클래스의 메서드를 단순화 한 표현식이다.


ex)

```java
(String a, String b) -> a.equals(b)
```

Collections. comparator

```java
Collecions.sort(cars, (car1, car2) -> car1.getCarPrice() - car2.getCarPrice())
```


### 함수 디스크립터 (Function Descriptor)
- 함수형 인터페이스의 추상 메서드를 설명해놓은 시그니처를 함수 디스크립터라고 한다.

Predicate<T>  |  T -> boolean
Consumer<T>  | T -> void
Function<T, R> | T -> R
Supplier<T>  |  () -> T
BiPredicate<L, R>  |  (L,R) -> boolean
BiConsumer<T, U>  |  (T,U) -> void
BiFunction<T, U, R>  |  (T, U) -> R


### 메서드 레퍼런스란?
객체의 래퍼런스

- 람다 표현식 부분에 기술되는 메서드를 이용해서 표현되며, 메서드의 이름만 전달
- ::를 붙이는 방식으로 메서드 레퍼런스를 표현한다.

```java
ClassName::static method
(String s) -> Integer.parseInt(s)
Integer::parseInt

        
ClassName::instance method
(String s) -> s.lowerCase()


object::instance method
(int count) -> obj.getTotal(count)
obj::getTotal


CkassName::new
() -> new Car()
```


## 리액티브 연산자 개요 및 생성자 연산자

RxJava의 연산자는 메서드 
에러처리, 유틸리티, 조건과 불린, 통지된 데이터 변환, 통지된 데이터 필터링, 통지된 데이터 집계


- interval
  - 완료 없이 계속 통지한다.
  - 호출한 스레드와는 별도의 스레드에서 실행된다.
  - polling 용도의 작업을 수행할 때 활용할 수 있다.

```java
public static void main(String[] args) {
    Observable.interbal(0L, 1000L, TimeUnit.MILLISECONDS)
        .map(num -> num + " count")
        .subscribe(data -> Logger.Log(LogType.ON_NEXT, data));
}
```

- range
  - range(n, m) n ~ n + m - 1

- timer
  - 지정한 시간이 지나면 0을 통지
  - 0을 통지하고 onComplete() 이벤트가 발생하여 종료
  - 호출한 스레드와는 별도의 스레드에서 실행
  - 특정 시간을 대기한 후에 어떤 처리를 하고자 할 때 활용할 수 있다.

- defer
  - 구독을 할 때마다 새로운 타임라인(Observable)이 생성된다.
  - 선언한 시점의 데이터를 통지하는 것이 아니라 호출 시점의 데이터를 통지한다.
  - 데이터 생성을 미루는 효과가 있기 때문에 최신 데이터를 얻고자할 때 활용할 수 있다.

- fromIterable
  - Iterable 인터페이스를 구현한 클래스를 파라미터로 받는다.
  - Iterable에 담긴 데이터를 순서대로 통지한다.

- fromFuture
  - Future 인터페이스는 자바 5에서 비동기 처리를 위해 추가된 동시성 API
  - 시간이 오래 걸리는 작업은 Future를 반환하는 ExcutorService에게 맡기고 비동기로 다른 작업을 수행할 수 있다.
  - Java 8 에서는 CompletableFuture 클래스를 통해 구현이 간결해졌다.



## 데이터 필터링 연산자

- filter
  - 전달 받은 데이터가 조건에 맞는지 확인 후, true일 때만 반환


- distinct
  - 이미 통지된 동일한 데이터가 있다면 이후의 동일한 데이터는 통지하지 않는다.

- take
  - 파라미터로 지정한 개수나 기간이 될 때까지 데이터를 통지한다.

- takeUntil 
- 첫번째 유형
  - 파라미터로 지정한 조건이 true가 될 때까지 데이터를 계속 통지한다.
- 두번째
  - 파라미터로 지정한 Observable이 최초 데이터를 통지할 떄까지 데이터를 통지


- skip
- 첫번째 유형
  - 파라미터로 지정한 숫자만큼 데이터를 건너 뛰고 통지
- 두번째 유형
  - 지정한 시간만큼 건너 뛰고 통지


## 변환 연산자 (1)

- map
  - 원본 Observable에서 통지하는 데이터를 원하는 값으로 변환 후 통지한다.

- flatMap
- 첫번째 유형
  - 원본 데이터를 원하는 값으로 변환 후 통지하는 것은 map 과 같다.
  - 데이터 하나로 여러개의 데이터를 담고 있는 새로운 Observale을 반환한다.
- 두번째 유형
  - 원본 데이터와 변환된 데이터를 조합해서 새로운 데이터를 통지

- concatMap
  - flatMap과 마찬가지로 받은 데이터를 변환하여 새로운 Observable로 반환
  - 데이터의 처리 순서 보장 하나가 끝나야 다음 Observable이 실행되므로 성능에는 영향을 줄 수 있다.

- switchMap
  - 새로운 데이터가 통지되면 현재 처리중이던 작업을 바로 중단함


## 변환 연산자 (2)

- groupBy
  - GroupedByObservable로 만든다.
  - 원본 Observable의 데이터를 그룹별로 묶는다기보다는 각각의 데이터들이 그룹에 해당하는 Key를 가지게 된다.

- toList
  - 통지 되는 데이터를 모두 리스트에 담겨서 한 번에 통지한다.
  - Single로 통지된다.

- toMap
  - 통지 되는 데이터를 모두 맵에 담겨서 한 번에 통지한다.
  - 첫 번째 반환값은 Map의 Key가 된다. (두 번째 optional 반환 값은 value)


## 데이터 결합 연산자

- merge
  - 다수의 Observable에서 통지된 데이터를 받아서 다시 하나의 Observable로 통지한다.
  - 통지 시점이 빠른 데이터부터 시점이 같을 경우 먼저 지정된 Observable부터 통지된다.

- concat
  - 하나의 Observable 통지가 끝나면 다음 Observable에서 연이어서 통지가 된다.

- zip
  - 각 Observable에서 통지된 데이터가 모두 모이면 각 Observable에서 동일한 index의 데이터로 새로운 데이터를 생성한 후 통지
  - 데이터 개수가 가장 적은 Observable의 통지 시점에 완료 시점을 맞춘다.

- combineLatest
  - 각 Observable에서 데이터를 통지할 때마다 모든 Observable에서 마지막으로 통지한 각 데이터를 함수형 인터페이스에 전달하고 새로운 데이터를 생성해 통지한다.

  
## 에러 처리 연산자

rxJava 프로그래밍에서는 try catch 가 아닌 onError로 처리

- onErrorReturn
  - 에러가 발생했을 때 에러를 의미하는 데이터로 대체할 수 있다.
  - onError 이벤트는 발생하지 않음

- onErrorResumeNext
  - 에러가 발생했을 때 에러를 의미하는 Observable로 대체할 수 있다.

- retry
  - 데이터 통지 중 에러가 발생했을 때, 데이터 통지를 재시도 한다.
  - 에러가 발생한 시점에 통지에 실패한 데이터만 다시 통지되는 것이 아니라 처음부터 다시 통지


## 유틸리티 연산

- delay 첫 번째 유형
  - 생산자가 데이터를 생성 및 통지를하지만 설정한 시간만큼 지연된 후에 제공한다.
- delay 두 번째 유형
  - 각각의 원본 데이터의 통지를 지연 시킨다.
  
- delaySubscription
  - 생산자가 데이터의 생성 및 통지 자체를 설정한 시간만큼 지연시킨다.

- timeout
  - 각각의 데이터 통지 시, 지정된 시간안에 통지가 되지 않으면 TimeoutException을 통지

- timeInterval
  - 각각의 데이터가 통지되는데 걸린 시간을 통지한다.
  - 통지된 데이터와 데이터가 통지되는데 걸린 시간을 소비자가 모두 처리할 수 있음

- materialize / dematerialize
  - 통지된 데이터와 통지된 데이터의 통지 타입 자체를 Notification 객체에 담고 통지함
  - Notification 객체를 원래의 통지 데이터로 변환


## 조건과 불린 연산자

- all
  - 통지되는 모든 데이터가 설정한 조건에 맞는지를 판단
  - single로 반환함

- amb
  - 최초 통지 시점이 가장 빠른 Observable 데이터만 전달
  - 나머지는 무시된다

- contains
  - 파라미터의 데이터가 Observable에 포함된어 있는지 판단
  - 결과갚을 한번만 통지하면 되기때문에 true/false single로 반환

- defaultIfEmpty
  - 통지할 데이터가 없을 경우 파라미터로 입력된 값을 통지한다.

- sequenceEqual
  - 두 개의 생산자의 데이터가 같음을 확인


## 데이터 집계 연산자

- count
  - 데이터의 총 개수를 통지

- reduce
  - 통지한 데이터를 이용해서 어떤 결과를 일정한 방식으로 합성한 후 최종 결과를 반환


