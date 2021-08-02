## Reactor - kakao Tech reactor 게시물 정리

Reactor는 Pivotal의 오픈소스 프로젝트, JVM위에서 동작하는 논블럭킹(소켓 관련 시스템 콜에 대하여 네트워크 시스템이 즉시 처리할 수 없는 경우라도 시스템콜이 바로 리턴되어 응용 프로그램이 block
되지 않게 한다.)
애플리케이션을 만들기 위한 리액티브 라이브러리이다.

Spring Framework 5 부터 리액티브 프로그래밍을 위해 지원되는 라이브러리이다. 최소 Java8에서 동작하며 RxJava와 많은 공통점이 있다.

### Mono와 Flux

> Mono는 0-1개의 결과만을 처리하귀 위한 Reactor의 객체이고, Flux는 0-N개인 여러 개의 결과를 처리하는 객체이다.
>
> Reactor를 사용해 일련의 스트림을 코드로 작성하다 보면 보통 여러 스트림을 하나의 결과를 모아줄 때 Mono를 쓰고, 각각의 Mono를 합쳐서 여러 개의 값을 처리하는
> Flux로 표현할 수도 있다.
>
> Mono와 Flux모두 Reactive Stream의 Publisher 인터페이스를 구현하고 있으며, Reactor에서 제공하는 연산자들의 조합을 통해
> 스트림을 표현할 수 있다.


바구니 예제

```java
public static void main(String[]args){
    final List<String> basket1=Arrays.asList(new String[]{"kiwi","orange","lemon","orange","lemon","kiwi"});
    final List<String> basket2=Arrays.asList(new String[]{"banana","lemon","lemon","kiwi"});
    final List<String> basket3=Arrays.asList(new String[]{"strawberry","orange","lemon","grape","strawberry"});
    final List<List<String>>baskets=Arrays.asList(basket1,basket2,basket3);
    final Flux<List<String>>basketFlux=Flux.fromIterable(baskets);
}
```

바구니에 값들을 새로운 Publisher로 바꿔줄 수 있는 연산자로는

- flatMap : 비동기로 동작할 때 순서를 보장하지 않음
- flatMapSequential : 순서를 보장, 들어오는 대로 구독하고 결과를 리턴
- concatMap : 순서를 보장, 스트림이 다 끝난 후에 그다음 넘어오는 값의 Publisher 스트림을 처리

```java
basketFlux.concatMap(basket->{
    final Mono<List<String>>distinctFruits=Flux.fromIterable(basket).distinct().collectList();
    final Mono<Map<String, Long>>countFruitsMono=Flux.fromIterable(basket)
            .groupBy(fruit->fruit) // 바구니로 부터 넘어온 과일 기준으로 group을 묶는다.
            .concatMap(groupedFlux->groupedFlux.count()
            .map(count->{
    final Map<String, Long> fruitCount=new LinkedHashMap<>();
            fruitCount.put(groupedFlux.key(),count);
            return fruitCount;
            }) // 각 과일별로 개수를 Map으로 리턴
            ) // concatMap으로 순서보장
            .reduce((accumulatedMap,currentMap)->new LinkedHashMap<String, Long>(){{
            putAll(accumulatedMap);
            putAll(currentMap);
            }}) // 그동안 누적된 accumulatedMap에 현재 넘어오는 currentMap을 합쳐서 새로운 Map을 만든다. // map끼리 putAll하여 하나의 Map으로 만든다.
    return Flux.zip(distinctFruits,countFruitsMono,(distinct,count)->new FruitInfo(distinct,count));
}).subscribe(System.out::println);
```
-> 총 2번 basket을 순회하고 특별히 스레드를 지정하지 않았기 때문에 동기, 블로킹 방식으로 동작

### 병렬로 두 스트림 합치기
Reactor나 RxJava 모두 동시성 지원을 위해 Scheduler를 제공, 적절한 Scheduler를 적절한 위치에 지정함으로써 동시성과 실행 순서를 관리할 수 있다.

subscribeOn을 이용하면 해당 스트림을 구독할 때 동작하는 스케줄러를 지정할 수 있다.

```java
basketFlux.concatMap(basket -> {
    final Mono<List<String>> distinctFruits = Flux.fromIterable(basket).log().distinct().collectList().subscribeOn(Schedulers.parallel());
    final Mono<Map<String, Long>> countFruitsMono = Flux.fromIterable(basket).log()
            .groupBy(fruit -> fruit) // 바구니로 부터 넘어온 과일 기준으로 group을 묶는다.
            .concatMap(groupedFlux -> groupedFlux.count()
                .map(count -> {
                    final Map<String, Long> fruitCount = new LinkedHashMap<>();
                    fruitCount.put(groupedFlux.key(), count);
                    return fruitCount;
                }) // 각 과일별로 개수를 Map으로 리턴
            ) // concatMap으로 순서보장
            .reduce((accumulatedMap, currentMap) -> new LinkedHashMap<String, Long>() { {
                putAll(accumulatedMap);
                putAll(currentMap);
            }}) // 그동안 누적된 accumulatedMap에 현재 넘어오는 currentMap을 합쳐서 새로운 Map을 만든다. // map끼리 putAll하여 하나의 Map으로 만든다.
            .subscribeOn(Schedulers.parallel());
    return Flux.zip(distinctFruits, countFruitsMono, (distinct, count) -> new FruitInfo(distinct, count));
}).subscribe(
    System.out::println,  // 값이 넘어올 때 호출 됨, onNext(T)
    error -> {
        System.err.println(error);
    countDownLatch.countDown();
    }, // 에러 발생시 출력하고 countDown, onError(Throwable)
    () -> {
        System.out.println("complete");
        countDownLatch.countDown();
    } // 정상적 종료시 countDown, onComplete()
);

```
병렬로 동작해도 baskets를 각각 순회하기 때문에 배보다 배꼽이 더 클 수 있다 - basket당 하나의 스트림만 공유하게 할 수는 없을까?

### Hot과 Cold
Cold는 각 Flux나 Mono를 subscribe 할 때마다 매번 독립적으로 새로운 데이터를 생성
default cold
하지만 Hot은 구독하기 전부터 데이터의 스트림이 동작할 수 있다.

```java
basketFlux.concatMap(basket -> {
    final Flux<String> source = Flux.fromIterable(basket).log().publish().autoConnect(2).subscribeOn(Schedulers.single());
    final Mono<List<String>> distinctFruits = source.publishOn(Schedulers.parallel()).distinct().collectList().log();
    final Mono<Map<String, Long>> countFruitsMono = source.publishOn(Schedulers.parallel())
            .groupBy(fruit -> fruit) // 바구니로 부터 넘어온 과일 기준으로 group을 묶는다.
            .concatMap(groupedFlux -> groupedFlux.count()
                    .map(count -> {
                        final Map<String, Long> fruitCount = new LinkedHashMap<>();
                        fruitCount.put(groupedFlux.key(), count);
                        return fruitCount;
                    }) // 각 과일별로 개수를 Map으로 리턴
            ) // concatMap으로 순서보장
            .reduce((accumulatedMap, currentMap) -> new LinkedHashMap<String, Long>() { {
                putAll(accumulatedMap);
                putAll(currentMap);
            }}) // 그동안 누적된 accumulatedMap에 현재 넘어오는 currentMap을 합쳐서 새로운 Map을 만든다. // map끼리 putAll하여 하나의 Map으로 만든다.
            .log();
    return Flux.zip(distinctFruits, countFruitsMono, (distinct, count) -> new FruitInfo(distinct, count));
}).subscribe(/* 생략.. */);
```

### 테스팅
- https://projectreactor.io/docs/core/snapshot/reference/#debug-activate



참고

- https://tech.kakao.com/2018/05/29/reactor-programming/
- https://projectreactor.io/docs/core/release/reference/