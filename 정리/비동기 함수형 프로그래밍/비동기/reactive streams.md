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
















