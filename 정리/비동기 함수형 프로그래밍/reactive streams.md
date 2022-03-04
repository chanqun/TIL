### Reactive Streams

[toby reactive](https://www.youtube.com/watch?v=8fenTR3KOJo)


RFP Reactive Functional Programming
-> 이벤트가 발생하면 대응하는 방식으로 작동하는 프로그래밍

Reactive Streams 표준 Java9 API

자바의 forEach는 iterator을 구현한 것이다.


Iterable <---> Observable (duality) 쌍대성  / 9 까지였음
Pull           Push

정보를 주고 받는 방향이 완전히 반대다

notifyObservers(i); // push
int i = it.next();  // pull


```java
IntObservable io = new IntObservable();
io.addObserver(ob);

io.run();
```

기존 Observable은 notify 밖에 없음 관리가 안됨
재시도 fallback 이런 것이 없음

Complete
Error 관리


[reactive document](http://www.reactive-streams.org)

org.reactivestreams
```java
Processor
Publisher
Subscriber
Sbuscription
```

```java
Sub,   Pub
pub.subscribe(sub)
```

Operators

Publisher -> [Data] -> Operator1 -> [Data2] -> Op2 -> [Data3] -> Subscriber

! 스프링에는 피료한 것만 상속할 수 있도록 adapterClass가 있음

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
### Toby의 주말 강의

http3 : grpc, rsocket

thread 여러 작업을 동시에 동작하는 concurrency

병렬은 물리적으로 여러개의 쓰레드에서 동시에 동작하는거고
ex) 10개를 돌리면 10개가 real time 으로 돈다.

동시성
ex) 회의도하고 커피도 시켜야한다. 동시적이지만 같이 하지는 않음

I/O cpu 사용하면서 전환에 문제가 있다.



```
thread hell in linkedin


밥먹고 나른한 14시 
thread pool 사용량이 급격히 늘어나 Latency

thread를 채우고 queue까지 채우고 에러가 반환된다.

서버를 늘려서 이런 문제를 해결할 수 있겠지만
비동기 프로그래밍을 통해 해결 가능하다.
 
cpu는 놀고 있는데 스레드가 부족할 때
스레드를 다른 작업을 할 수 있게 즉시 해제되게 해줄 수 있음
```

반응이 필요하면 동기로 하고 polling 하는 것이 좋다.

// Reactive / RxJava / WebFlux / Reactor

1. reactive chain 작업 흐름을 만들고
2. subscribe 구독

요청해서 데이터를 만들면 COLD
주식 시장 데이터가 HOT

// subroutine: call -> response
// coroutine: call -> suspend -> resume -> response





