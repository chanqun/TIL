# rxJava2


## Processor와 Subject

- Processor는 Reactive Streams에서 정의한 Publisher 인터페이스와 Subscriber 인터페이스를 둘 다 상속한 확장 인터페이스이다.
- Processor는 Hot Publisher이다.

- Subject는 Processor와 동일한 기능을 하나 배압 기능이 없는 추상 클래스이다.
    - Publish, Async, Behavior, Replay : Processor, subject가 있다.


## Processor / Subject

- PublishSubject
  - 구독한 이후에 통지 된 데이터만 받을 수 있따.
  - 데이터 통지가 완료 된 이후에 소비자가 구독하면 완료 또는 에러 통지를 받는다.
  
- AsyncSubject
  - 완료 전까지 아무것도 통지하지 않고 있다가 완료했을 때 마지막으로 통지한 테이터와 완료만 통지한다.
  - 완료 후에 구독한 소비자라도 마지막으로 통지된 데이터와 완료를 통지 받는다.

- BehaviorSubject
  - 구독 시점에 이미 통지된 데이터가 있다면 이미 통지된 데이터의 마지막 데이터를 전달 받은 후, 구독 이후에 통지된 데이터를 전달 받는다.
  - 처리가 완료된 이후에 구독하면 완료나 에러 통지만 전달 받는다.

- ReplaySubject
  - 구독 시점에 이미 통지된 데이터가 있다면 이미 통지된 데이터 중에서 최근 통지된 데이터를 지정한 개수만큼 전달 받은 후, 구독 이후에 통지된 데이터를 전달 받는다.


## 물리적인 쓰레드와 논리적인 쓰레드

- 물리적인 쓰레드는 하드웨어와 관련, 논리적인 쓰레드는 소프트웨어와 관련
- 물리적인 쓰레드는 물리적인 코어를 논리적으로 쪼갠 논리적 코어

- 논리적인 쓰레드는 프로세스 내에서 실행되는 세부 작업의 단위

병렬성 core, 동시성 thread


## 스케줄러란?
- rxJava에서의 스케줄러는 rxJava 비동기 프로그래밍을 위한 쓰레드 관리자
- 어떤 스레드에서 무엇을 처리할 지에 대해서 제어할 수 있다.

### 스케줄러의 종류
Schedulers.io()
- 쓰레드 풀에서 쓰레드를 가져오거나 가져올 쓰레드가 없으면 새로운 쓰레드를 생성한다.

Schedulers.computation()
- 논리적인 연산 처리 시, 사용하는 스케줄러
- CPU 코어의 물리적 쓰레드 수를 넘지 않는 범위에서 쓰레드를 생성한다.

Schedulers.newThread()
- 요청시마다 새로운 쓰레드를 생성

```java
.subscribeOn(Schedulers.io())
.observeOn(Schedulers.computation())
```

## 스케줄러의 종류 2
Schedulers.trampoline()
- 현재 실행되고 있는 쓰레드에 큐를 생성하여 처리

Schedulers.single()
- 단일 쓰레드 생성

Schedulers.from(executor)
- executor를 사용해서 생성한 쓰레드를 사용한다.


## rxJava의 디버깅 문제점과 대안

- 선언적 프로그래밍 방식이기 떄문에 데이터의 상태 변화를 확인하기 위한 디버깅이 쉽지 않다.
- 비동기 프로그래밍이기 때문에 실행 시, 항상 같은 결과가 나온다는 보장을 할 수 없다.
- 함수형 프로그래밍의 특성상 부수 효과는 소비자쪽에서 처리하는 것이 맞지만 doXXX 함수는 예외다.
- 전달 받은 데이터를 처리하기 전 원 데이터의 상태나 변환 및 필터링 등으로 가공되는 시점의 데이터 상태를 doXXX 함수를 통해 쉽게 파악할 수 있다.

- doOnSubscribe
  - 구독 시작 시, 지정된 작업을 처리할 수 있다.

- doOnNext
  - 생산자가 데이터를 통지하는 시점에, 지정된 작업을 처리할 수 있다.
  
- doOnComplete
  - 생산자가 완료를 통지하는 시점에, 지정된 작업을 처리할 수 있다.

- doOnError
  - 생산자가 에러를 통지하는 시점에, 지정된 작업을 처리할 수 있다.

- doOnEach
  - notification 객체에서 전부 제공
  - doOnNext, doOnComplete, doOnError를 한 번에 처리할 수 있다.

- doOnCancel / doOnDisposal
  - 소비자가 구독을 해지하는 시점에, 지정된 작업을 처리할 수 있다.
  - 완료나 에러로 종료 될 경우에는 실행되지 않는다.

- doAfterNext
  - 생산자가 통지한 데이터가 소비자에 전달된 직후 호출되는 함수

- doOnTerminate
  - 완료 또는 에러가 통지 될 때 호출 되는 함수

- doAfterTerminate
  - 완료 또는 에러가 통지된 후 호출 되는 함수

- doFinally
  - 구독이 취소 된 후, 완료 또는 에러가 통지된 후 호출되는 함수

- doOnLifecycle
  - 소비자가 구독할 떄 또는 구독 해지할 때 호출되는 함수


## 테스트

- 테스트를 위한 blockingXXX 함수
  - 비동기 처리 결과를 테스트하려면 현재 쓰레드에서 호출 대상 쓰레드의 실행 결과를 반환 받을때까지 대기할 수 있어야한다.
- rxJava에서는 현재 쓰레드에서 호출 대상 쓰레드의 처리 결과를 받을 수 있는 blockingXXX 함수를 제공


- blockingFirst
  - 생산자가 통지한 첫번쨰 데이터를 반환한다.

- blockingLast
  - 생산자가 통지한 마지막 데이터를 반환한다.