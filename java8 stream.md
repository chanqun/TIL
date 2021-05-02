## 스트림

자바 8에서 추가한 스트림(Streams)은 람다<sup>1)</sup>를 활용할 수 있는 기술 중 하나이다.
이전에는 for 또는 foreach 문을 돌면서 요소 하나씩 꺼내서 다루는 방법이었다.
간단한 경우라면 상관없지만 로직이 복잡해지면 로직이 복잡해지고, 메소드를 나눌 경우 루프를 여러 번 도는 경우가 발생했다.



스트림이란?
데이터의 흐름으로 정의할 수 있다.
즉 배열과 컬렉션을 함수형으로 처리할 수 있다.



장점으로는 병렬처리(multi-threading) <sup>2)</sup>가 가능하다.

stream 사용이유? 
Collection을 사용하는 코드를 반복적으로 구현해야 했음
병렬적으로 Collection을 처리하기 어려웠기 때문이다.
Divide and Conquer알고리즘이 성능을 높일 수 있는 방법 중 하나아기 때문에 작업들을 병렬적으로 수행하여 작업의 효율을 높일 수 있기 때문에 사용 --> parallelStream()

> 확인사항 
> parallelStream만 쓰면 되지 왜 stream도 사용하는 것일까?
> parallel stream을 남용할 경우 성능이 떨어지는 경우가 있다 ??!





### 스트림을 사용한 간단한 예시

```java
import java.util.*;
import java.util.stream.Collectors;

public class StreamTest {
    public static void main(String[] args) {
        int arr[] = {8, 4, 10, 6, 6, 3};
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();

        //1. 스트림을 사용하지 않을 경우
        for (int i = 0; i < arr.length; i++) { //배열을 set에 넣기
            set.add(arr[i]);
        }

        Iterator<Integer> iterator = set.iterator(); //set을 iterator안에 담기

        while(iterator.hasNext()){
            list.add(iterator.next());
        }

        list.sort(Comparator.reverseOrder()); //역순으로 정렬

        System.out.println("스트림을 사용하지 않은 출력 " + list.toString());

        //2. 스트림을 사용하는 경우
        System.out.println("스트림을 이용한 출력: " +
                Arrays.stream(arr).boxed() //스트림생성
                        .distinct() //중복제거
                        .sorted(Comparator.reverseOrder()) //역정렬
                        .collect(Collectors.toList()) //List로 반환
        );
    }
}

```

위 예제로 알 수 있듯이 스트림은 배열이나 컬렉션(List, Set, Map)으로 원하는 값을 얻을 때 for문 도배를 방지할 수 있다.



### 스트림은 선언(생성), 가공, 반환(결과) 세 부분으로 이루어진다.

선언 : 배열, 컬렉션등을 스트림 형태로 만든다.

```java
// 배열 스트림
String[] stringArr = new String[]{"a", "b", "c", "d"};
Stream<String> stream = Arrays.stream(stringArr);
Stream<String> streamOfPart = Arrays.stream(stringArr, 1, 3)
    
// 컬렉션 스트림
List<String> stringList = Arrays.asList("a", "b", "c", "d");
Stream<String> stream = stringList.stream();
Stream<String> parallelStream = stringList.parallelStream();

// 빈 스트림
Stream.empty();

// Stream.builder()
Stream<String> streamBuilder = 
    Stream.<String>builder()
    	.add("a").add("b").add("c")
    	.build();

// Stream.generate()
Stream<String> streamGenerated =
    Stream.generate(() -> "hi").limit(5);

// Stream.iterate()
Stream<Integer> streamIterated = 
    Stream.iterate(10, n -> n + 3).limit(3);

// primitive stream
IntStream intStream = IntStream.range(1, 4); // [1, 2, 3]
LongStream longStream = LongStream.rangeClosed(1, 4); //[1, 2, 3, 4]

Stream<Integer> intStreamBox = IntStream.range(1, 4).boxed();

DoubleStream doubles = new Random().doubles(3); //난수 3개

// char stream
Stream<String> stringStream =
    Pattern.compile(", ").splitAsStream("A, B, C");

// 파일 스트림
Stream<String> fileStream =
    Files.lines(Paths.get("input.txt"),
               Charset.forName("UTF-8"));

// 병렬 스트림 Parallel Stream
stream 대신 parallelStream 메소드를 사용한다.
    
// 스트림 연결하기 Stream.concat
Stream<String> stream1 = Stream.of("A", "B", "C");
Stream<String> stream2 = Stream.of("D", "E", "F");
Stream<String> concat = Stream.concat(stream1, stream2);

```



가공 : 스트림을 필요한 형태로 가공한다.

```
// Filtering
필터는 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업이다.

// Mapping
맵은 스트림 내 요소들을 하나씩 특정 값으로 변환해준다. 이 때 값을 변환하기 위한 람다를 인자로 받는다.
flatMap -> 중첩 구조를 제거한 후 작업할 수 있게 해준다.

// Sorting
Comparator을 이용해서 정렬하는 것

//Iterating
peek은 스트림 내 요소들 각각에 특정 작업을 수행할 뿐 결과에 영향을 미치지는 않는다.
작업 중간에 사용할 수 있다.
```



반환 : 가공한 값을 원하는 형태로 가져오기

```java
// Calculation
최소, 최대, 합, 평균 등 기본형 타입으로 결과를 만들 수 있다.

// reduction
reduce는 세 가지의 파라미터를 받을 수 있음
- accumulator : 각 요소를 처리하는 계산 로직, 각 요소가 올 때마다 중간 결과를 생성
- identity : 계산을 위한 초기값으로 스트림이 비어서 계산할 내용이 없더라도 이 값은 리턴
- combiner : 병렬 스트림에서 나눠 계산한 결과를 하나로 합치는 동장하는 로직

// Collecting
Collectors.toList() // 스트림에서 작업한 결과를 담은 리스트로 반환

Collectors.joining() // 스트림에서 작업한 결과를 하나의 스트링으로 이어 붙일 수 있음
- delimiter : 각 요소 중간에 들어가 요소를 구분시켜주는 구분자
- prefix : 결과 맨 앞에 붙는 문자
- suffix : 결과 맨 뒤에 붙는 문자

Collectors.averageingInt()
숫자 값의 평균을 낸다.

Collectors.summingInt()
숫자값의 합을 낸다.

Collectors.summarizingInt()
합계와 편균 모두 필요할 때

Collectors.groupingBy()
특정 조건으로 요소들을 그룹지을 수 있다. 
여기서 받는 인자는 함수형 인터페이스 Function이다.

Collectors.partitioningBy()
함수형 인터페이스 Predicate를 받는다. Predicat인자를 받아서 boolean 값을 리턴

Collectors.collectingAndThen()
특정 타입으로 결과를 collect 한 이후에 추가 작업이 필요한 경우에 사용

Collectors.of()
직접 만들 수 있다.

// Matching
anyMatch : 하나라도 조건을 만족하는 요소가 있는지
allMatch : 모두 조건을 만족하는지
noneMatch : 모두 조건을 만족하지 않는지

// Iterating
forEach는 요소를 돌면서 실행되는 최종 작업 peek과는 중간 작업과 최종 작업이라는 차이가 있음
```



그렇다면 스트림의 단점은 무엇일까?

1. 디버그가 힘듬
   - 에러가 발생한다면 일반 코드의 경우 값이 잘못되기 직전에 디버그를 걸어놓으면 확인할 수 있음 하지만 스트림은 한 번에 수행되기 때문에 처음부터 전부 확인해야 한다.
2. 재사용이 불가능
   - 스트림은 한 번 쓰면 close되기 때문에 
     한 번 정의해놓고 계속 사용이 불가능하다.



### Stream Example













<sup>1)</sup>람다식이란?
람다식이란 "식별자 없이 실행가능한 함수"
함수인데 함수를 따로 만들지 않고 코드한줄에 함수를 써서 그것을 호출하는 방식

<sup>2)</sup>병렬처리란?
하나의 작업을 둘 이상의 작업으로 잘게 나눠서 동시에 진행하는 것

