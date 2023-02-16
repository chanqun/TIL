
## 1주차 JVM

### java compile option
javac 입력시
-target 1.8 주면 하위 버전에서도 사용 가능하게 할 수 있다.

### JIT
runtime 영역에 JIT(just-in-time) 컴파일러 스레드가 돌고 있다가 interpreter가 자주 사용하는 것을 기계어로 캐싱한다.
다시 호출되면 JIT이 기계어로 컴파일러 한 것을 사용한다. 
(client-mode, server-mode) 1.8 부터는 기본적으로 layered 적용됨

### JVM 구성 요소
- 클래스 로더(Class Loader)
- 실행 엔진(Execution Engine)
  - 인터프리터(Interpreter)
  - JIT 컴파일러(Just-in-Time)
  - 가비지 콜렉터(Garbage collector)
- 런타임 데이터 영역 (Runtime Data Area)
  - Thread(PC Register, jvm, stack, native method stack) : 프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다. 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장한다.
  - Heap(Eden, Survivor 0, Survivor 1, Old, Permanent)
    - Permanent : 생성된 객체들의 정보의 주소값이 저장된 공간이다. 클래스 로더에 의해 load 되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다.
      Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
  - Method Area(Runtime Constant Pool) : 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간, 상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.


### JDK, JRE
9부터는 jdk만 나옴
jdk에 jre 들어있음

### int a = 10;
한 줄 처럼 보이지만 javap로 보면 두 줄이기 때문에 멀티 스레드 환경에서 문제가 발생할 수 있다.



## 2주차 데이터 타입

### primitive type
boolean(1byte), char(2byte), byte(byte), short(2byte), int(4byte), long(8byte), float(4byte), double(8byte)

### Literal
1_000_000, 0, 0x, 0b, L, E-4 

### reference type
String, Array, Collections, Class, Interface

### 변수의 스코프와 라이프타임 - 로딩시점이 다름
- 클래스 변수 : static은 클래스 로딩 시점
- 인스턴스 변수
- 지역 변수

### 배열
배열이란 선형 자료 구조 중 하나로, 동일한 타입의 연관된 데이터를 메모리에 연속적으로 저장하여 하나의 변수에 묶어서 관리하기 위한 자료구조이다.

### 타입추론
지역변수를 선언할 때 초깃값을 통하여 데이터 타입을 추론하게 한다.
var 는 초기값이 있는 지역 변수로만 선언 가능하다.

## 3주차 연산자

### 산술 연산자

### 비트 연산자
&,|,^,~,<<,>>(/2 n번과 같음),>>>(음수를 고려하지 않고 양수만)

### 관계 연산자
<,><= ,>=, ==, !=

### 논리 연산자
&&, || , !

### instanceof

### assignment operator
+=

### 화살표 연산자
->
화살표 연산자는 익명 메소드의 매개변수와 리턴변수를 통해 만들어진다. 더 정확히 말하자면 익명 메소드가 아니라 람다를 통해 만들어진다.

### 3항 연산자
### 연산자 우선 순위
단항 > 산술 > 비교 > 논리 > 삼항 > 대입

### java 13 switch 연산자
```java
int result = switch (mode) {
    case "a":
        yield 1;
    default:
        yield -1;
};
```

```java
int mid = start + (end - start) / 2;
int mid = (start + end) >>> 1; //양수만 됨
```

배열에서 한 번만 들어간 숫자 찾기 XOR로 풀기 교환법칙 성립함 O(n)으로 풀 수 있음


## 4주차 제어문

### 제어문
if, switch, for, while, do-while, for each

### LinkedList, Stack, Queue

offer, add
poll, remove
peek, element

익셉션 에러 유무 차이: 일관성을 유지하는 것이 중요하다.

### Replace Conditional with Polymorphism


## 5주차 클래스

자바는 call by value 0x001 이런것 넘어감

메소드같은 reference가 넘어가는게 아님!

### 클래스 정의
인스턴스, 클래스(정적)

초기화 블록(클래스, 인스턴스)
```java
static {} // 클래스 초기화
```

### 객체 만드는 방법
new 키워드는 새 객체에 메모리를 할당하고 해당 메모리에 대한 참조값을 반환하여 클래스를 인스턴스화한다.
super()은 숨겨져 있다.

### this()
해당 클래스 생성자를 호출할 수 있다.


인스턴스 생성을 막을떄는 abstract 상속을 막으려면 final을 쓴다.

### etc
public 전체
protected 자손
default 같은 패키지
private 같은 클래스


static
변수, 메서드는 객체가 아닌 클래스에 속한다.

final
클래스 앞에 붙으면 해당 클래스는 상속될 수 없다.
변수 또는 메서드 앞에 붙으면 수정되거나 오버라이딩 될 수 없다.

새롭게 할당하는 것을 막는 것임

abstract
클래스 앞에 붙으면 추상 클래스가 되어 객체 생성이 불가하고, 접근을 위해선 상속받아야 한다.
변수 앞에 지정할 수 없다. 메서드 앞에 붙는 경우는 오직 추상 클래스 내에서의 메서드밖에 없으며 해당 메서드는 선언부만 존재하고 구현부는 상속한 클래스 내 메서드에 의해 구현되어야 한다. 상속과 관련된 내용은 6주차에 다룰 예정이다.

transient
변수 또는 메서드가 포함된 객체를 직렬화할 때 해당 내용은 무시된다.

synchronized
메서드는 한 번에 하나의 쓰레드에 의해서만 접근 가능하다.

volatile
해당 변수의 조작에 CPU 캐시가 쓰이지 않고 항상 메인 메모리로부터 읽힌다.

### reflection
class에 대부분이 reflection
Class.forName("com.a.x").class.getDeclaredFields(); 

## 6주차 상속

상속 - 점선, 구현 - 실선

### 자바는 다중 상속을 허용하지 않음.

### super
super 사용하면 서브클래스가 수퍼클래스에 접근이 가능하다.

### 메서드 오버라이딩
수퍼클래스가 가지고 있는 메서드를 서브클래스에서 새롭게 다른 로직으로 정의하고 싶을 떄 사용하는 문법

### 다이나믹 디스패치
컴파일 타임에는 메서드의 클래스타입이 정해져있지 않지만 런타임에 정해져서 메서드를 호출하는 것을
동적 dispatch 라고 한다.

바이트코드 상으로는 어떤 인스턴스에 invokevirtual을 호출하는지 모르긴 한다.

### 더블 디스패치
visitor 패턴

```java
interface Post {
    void postOn(SNS sns);
}

class Text implements Post {
    @Override
    public void postOn(SNS sns) {
        sns.post(this);
    }
}

class Picture implements Post {
    @Override
    public void postOn(SNS sns) {
        sns.post(this);
    }
}

interface SNS {
    void post(Text text);

    void post(Picture picture);
}

class FaceBook implements SNS {
    @Override
    public void post(Text text) {
        System.out.println("facebook " + "text");
    }

    @Override
    public void post(Picture picture) {
        System.out.println("facebook " + "picture");
    }
}

class Instagram implements SNS {
    @Override
    public void post(Text text) {
        System.out.println("Instagram " + "text");
    }

    @Override
    public void post(Picture picture) {
        System.out.println("Instagram " + "picture");
    }
}

```

## 7주차 패키지

FQCN(Fully Qualified Class Name)

- java 자바 기본
- javax 자바 확장
- org 비영리 (오픈소스)
- com 일반적 영리 (회사)


### classpath
JVM이 프로그램을 실행할 때, 클래스파일을 찾는 데 클래스 패스를 사용한다.
javac, java때 다 사용가능

(내가 읽으려는 파일이 부모한테 있는지 묻고 쓴다.)
- Bootstrap Class Loader - 최상위 클래스 로더, jre/lib/rt.jar String이나 object 같은 것을 메모리에 적재해줌
- Extension Class Loader - java.ext.dirs 환경 변수로 지정된 폴더에 있는 클래스 파일을 로딩
- System Class Loader - 우리가 만든 class를 메모리에 올리는 역할을 한다. classpath 기준으로 로드
  

interface 에서 변수 선언하면
static final 붙음 -> 안티패턴임


## 9주차 예외

Runtime은 복구가 어려운 경우
Checked는 우리가 무언가 처리를 할 수 있는 경우

기존에 익셉션을 잘 참고해서 추가하자,
최초에 예외가 아니고 변환하는 것이면 e를 잘 담아서 던지자
예외는 stackTrace를 담기 때문에 비싼 작업

### try with resources - kotlin은 use
catch 블럭에 자동으로 생성해주는 것임 Throwable

hello > close > finally

## 10주차 멀티스레드 프로그래밍

### Thread 클래스와 Runnable 인터페이스

JVM은 다음중 하나가 발생할때 까지 쓰레드를 유지한다.

Runtime 클래스의 종료 메소드가 호출되었으며 보안관리자(security manager)가 종료 조작이 발생하도록 허용할 때
데몬 쓰레드가 아닌 모든 쓰레드는 실행된 후 run() 메소드의 작업이 끝나거나 run 메소드 이외에서 예외를 throw 했을 때 종료된다.

Thread 상속 받아야하는 경우 > run 이외에 메소드를 override 할 필요가 있는 경우
아니면 implements Runnable

> Thread는 서버의 리소스를 극한으로 사용할 때 사용한다. (우리가 쓰는 서버 컨테이너가 대신 해줌 : tomcat, jetty, netty, undertow)
> connection pool (요즘은 코어 개수 만큼 만들고 blocking 될 만 한 것들은 asynchronous 처리함)

daemon thread는 부모 끝나면 끝남

한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것을 쓰레드의 동기화(synchronization)

atomic 한 것들은 do while 코드가 들어있음
volatile 은 메인 메모리에서 가져옴 안 쓰면 cache 되는 것을 가져옴 ! 한 thread만 write하는 가정이 있어야 함 

### critical path
동시에 실행하는 것 중 가장 오래 걸리는 것

### race condition
공유하는 자원이 있고 접근하는 순서에 따라 결과가 달라지는 경우

visualVM 에서 Thread Dump 제공 
메모리 스냅샷 heap dump -> jvm 메모리 만큼 내 컴퓨터에도 메모리 있긴 해야함