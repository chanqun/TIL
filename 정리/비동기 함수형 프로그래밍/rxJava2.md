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





