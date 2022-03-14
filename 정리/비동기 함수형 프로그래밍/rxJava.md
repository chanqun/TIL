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
```












  






