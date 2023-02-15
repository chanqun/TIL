
## 1주차

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



## 2주차

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

## 3주차

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


## 4주차

### 제어문
if, switch, for, while, do-while, for each

### LinkedList, Stack, Queue

offer, add
poll, remove
peek, element

익셉션 에러 유무 차이: 일관성을 유지하는 것이 중요하다.

### Replace Conditional with Polymorphism


## 5주차

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

## 6주차

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


