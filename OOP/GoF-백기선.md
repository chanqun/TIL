
## 객체 생성 관련

### 싱글톤 패턴

인스턴스를 오직 한개만 제공하는 클래스

- 시스템 런타임, 환경 세팅에 대한 정보 등, 인스턴스가 여러개 일 때 문제가 생길 수 있는 경우가 있다.
인스턴스를 오직 한개만 만들 제공하는 클래스가 필요하다.


```java
double checked locking으로 효율적인 동기화 블럭 만들기
private static volatile Singleton instance;

public static Singleton getInstance() {
    if (instance == null) {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
    }
    return singleton;
}

ㅡㅡㅡㅡ
static inner 클래스 사용하기
private static class SingletonHolder {
    public static final Singleton instance = new Singleton();
}

public static Singleton getInstance() {
    return SingletonHolder.instance;
}

```

싱글톤이 깨지는 경우
- 리플렉션을 사용하여 private 생성자를 호출하는 경우
    - ENUM을 사용하여 막을 수 있다, 단점 미리 만들어 진다
- 직렬화를 사용하여 인스턴스를 생성하는 경우
    - 대응 방안 : readResolve() 메서드를 구현하여 싱글톤 인스턴스를 반환하도록 한다.
- 멀티 스레드 환경에서 동시에 인스턴스를 생성하는 경우

- 스프링에서 빈의 스코프 중에 하나로 싱글톤 스코프
- 자바 java.lang.Runtime 클래스
- 다른 디자인 패턴 (빌더, 퍼사드, 추상 팩토리 등) 구현체의 일부로 쓰이기도 한다.


### 팩토리 메소드 패턴
구체적으로 어떤 인스턴스를 만들지는 서브 클래스가 정한다.
- 다양한 구현체가 있고, 그중에서 특정한 구현체를 만들 수 있는 다양한 팩토리를 제공할 수 있다.

OpenClose 원칙 확장에 대해서는 열려있고, 수정에 대해서는 닫혀있다.

- 단순한 팩토리 패턴
    - 매개변수의 값에 따라 또는 메소드에 따라 또는 메소드에 각기 다른 인스턴스를 리턴하는 단순한 버전의 팩토리 패턴
    - java.lang.Calendar 또는 java.lang.NumberFormat 클래스
- 스프링 BeanFactory
    - Object 타입의 Product를 만드는 BeanFactory라는 Creator

### 추상 팩토리 패턴

서로 관련있는 여러 객체를 만들어주는 인터페이스
- 구체적으로 어떤 클래스의 인스턴스를 사용하는지 감출 수 있다. (초점이 클라이언트쪽에 있음)

팩토리 메소드 vs 추상 팩토리

- 둘 다 구체적인 객체 생성 과정을 추상화한 인터페이스를 제공

- 관점이 다르다
    - 팩토리 메소드 패턴은 팩토리를 구현하는 방법에 초점을 둔다.
    - 추상 팩토리 패턴은 팩토리를 사용하는 방법에 초점을 둔다.

- 목적이 다르다.
    - 팩토리 메소드 패턴은 구체적인 객체 생성 과정을 하위 또는 구체적인 클래스로 옮기는 것이 목적
    - 추상 팩토리 패턴은 관련있는 여러 객체를 구체적은 클래스에 의존하지 않고 만들 수 있게 해주는 것이 목적

DocumentBuilderFactory - factory가 제공하는 추상적인 메소드를 통해 작동한다.

### 빌더 패턴
동일한 프로세스를 거쳐 다양한 구성의 인스턴스를 만드는 방법
- 객체를 만드는 프로세스를 독립적으로 분리할 수 있다.

클라이언트에서는 기존에 만들어진 코드를 사용

장점
- 만들기 복잡한 객체를 순차적으로 만들 수 있음, 다른 interface를 순서를 강제할 수 있음

단점
- 구조가 복잡하다

ex) StringBuilder, UriComponent


### 프로토타입 패턴
기존 인스턴스를 복제하여 새로운 인스턴스를 만드는 방법
- 기존 객체를 응용해서 만들때, 인스턴스를 만들 때 자원이 많이 드는 경우

implements Cloneable 해야함 java가 제공하는 clone 기능을 사

shallow copy - 얕은 복사 (기본 제공) 기존 reference를 똑같이 가리키게 됨

clone 부분을 임의로 변경해서 - deep copy 해도 됨

modelMapper는 reflection

### 어댑터 패턴

기존 코드를 클라이언트가 사용하는 인터페이스의 구현체로 바꿔주는 패턴
- 클라이언트가 사용하는 인터페이스를 따르지 않는 기존 코드를 재사용할 수 있게 해준다.

- 장점
    - 기존 코드를 변경하지 않고 원하는 인터페이스 구현체를 만들어 재사용할 수 있다.
    - 기존 코드가 하던 일과 특정 인터페이스 구현체로 변환하는 작업을 각기 다른 클래스로 분리하여 관리할 수 있다.
- 단점
    - 새 클래스가 생겨 복잡도가 증가할 수 있다. 경우에 따라서는 기존 코드가 해당 인터페이스를 구현하도록 수정하는 것이 좋은 선택이 될 수 있다.

OCP, SRP

DispatcherServlet -> HandlerAdapter (각기 다른 형태에 따라 다르게 처리해야하기 때문에)

### 브릿지 패턴
추상적인 것과 구체적인 것을 분리하여 연결하는 패턴
- 하나의 계층 구조일 때 보다 각기 나누었을 때 독립적인 계층 구조로 발전 시킬 수 있다.

- 장점
    - 추상적인 것과 구체적인 것을 분리하여 각각 독립적으로 확장할 수 있다.
    - 추상적인 코드와 구체적인 코드를 분리할 수 있다.
- 단점
    - 계층 구조가 늘어나 복잡도가 증가할 수 있다.

ex) LoggerFactory, jdbcTemplate

### 컴포짓 패턴
그룹 전체와 개별 객체를 동일하게 처리할 수 있는 패턴
- 클라이언트 입장에서는 전체나 부분이나 모두 동일한 컴포너트로 인식할 수 있는 계층 구조를 만든다.

- 장점
    - 복잡한 트리 구조를 편리하게 사용할 수 있다.
    - 디형성과 재귀를 활용할 수 있다.
    - 클라이언트 코드를 변경하지 않고 새로운 엘리먼트 타입을 추가할 수 있다.
- 단점
    - 트리 만들어야하기 때문에 (공통된 인터페이스를 정의해야 하기 때문에) 지나친 일반화 해야하는 경우도 생길 수 있다.

### 데코레이터 패턴
기존 코드를 변경하지 않고 부가 기능을 추가하는 패턴
- 상속이 아닌 위임을 사용해서 보다 유연하게(런타임에) 부가 기능을 추가하는 것도 가능하다.

- 장점
    - 새로운 클래스를 만들지 않고 기존 기능을 조합할 수 있다.
    - 컴파일 타임이 아닌 런타임에 동적으로 기능을 변경할 수 있다.
- 단점
    - 데코레이터를 조합하는 코드가 복잡할 수 있다.

Collections.checkedList(list, Book.class), 서블릿 요청 또는 응답 랩퍼


### 퍼사드 패턴
복잡한 서브 시스템 의존성을 최소화하는 방법
- 클라이언트가 사용해야 하는 복잡한 서브 시스템 의존성을 간단한 인터페이스로 추상화 할 수 있다.

mocking도 쉬움

- 장점
    - 서브 시스템에 대한 의존성을 한 곳으로 몰아 넣을 수 있다.
- 단점
    - 퍼사드 클래스가 서브 시스템에 대한 모든 의존성을 가지게 된다.

### 플라이웨이트 패턴

객체를 가볍게 만들어 메모리 사용을 줄이는 패턴
- 자주 변하는 속성과 변하지 않는 속성을 분리하고 재사용하여 메모리 사용을 줄일 수 있다.

- 장점
    - 메모리 사용을 줄일 수 있다.
- 단점
    - 코드의 복잡도가 증가한다.

Integer 자주쓰는 값을 caching 함

### 프록시 패턴
특정 객체에 대한 접근을 제어하거나 기능을 추가할 수 있는 패턴

- 초기화 지연, 접근 제어, 로깅, 캐싱 등 다양하게 응용해 사용할 수 있다.

- 장점
    - 기존 코드를 변경하지 않고 새로운 기능을 추가할 수 있다.
    - 기존 코드가 해야 하는 일만 유지할 수 있다.
    - 기능 추가 및 초기화 지연 등으로 다양하게 활용할 수 있다.

- 단점
    - 코드의 복잡도가 증가한다.

### 책임 연쇄 패턴
요청을 보내는 쪽과 요청을 처리하는 쪽을 분리하는 패턴
- 핸들러 체인을 사용해서 요청을 처리한다. (클라이언트가 구체적인 핸들러 타입을 모르게 한다)

- 장점
    - 클라이언트가 구체적인 핸들러 타입을 모르게 한다.
    - 핸들러를 추가하거나 변경하기 쉽다.
    - 핸들러를 조합해서 사용할 수 있다.
- 단점
    - 핸들러의 순서를 신경써야 한다.
    - 디버깅이 어려워짐

### 커맨드 패턴
요청을 캡슐화하여 호출자와 수신자를 분리하는 패턴

- 장점
    - 기존 코드를 변경하지 않고 새로운 커맨드를 만들 수 있따.
    - 수신자의 코드가 변경되어도 호출자의 코드는 변경되지 않는다.
    - 커맨드 객체를 로깅, DB에 저장, 네트워크로 전송 하는 등 다양한 방법으로 활용할 수도 있다.

- 단점
    - 코드가 복잡하고 클래스가 많아진다.

### 인터프리터 패턴
자주 등장하는 문제를 간단한 언어로 정의하고 재사용하는 패턴
- 반복되는 문제 패턴을 언어 또는 문법으로 정의하고 확장할 수 있다.

- 장점
    - 자주 등장하는 문제 패턴을 언어와 문법으로 정의할 수 있따.
    - 기존 코드를 변경하지 않고 새로운 Expression을 추가할 수 있다.
- 단점
    - 복잡한 문법을 표현하려면 Expression과 Parser가 복잡해진다.


### 이터레이터 패턴
집합 객체 내부 구조를 노출시키지 않고 순회 하는 방법을 제공하는 패턴

- 집합 객체를 순회하는 클라이언트 코드를 변경하지 않고 다양한 순회 방법을 제공할 수 있다.

StAX - Streaming API for XML

### 중재자(메디에이터) 패턴
여러 객체들이 소통하는 방법을 캡슐화하는 패턴
- 여러 컴포넌트같의 결합도를 중재자를 통해 낮출 수 있다.

guest -> front desk -> cleaning, restaurant

- 장점
    - 컴포넌트 코드를 변경하지 않고 새로운 중재자를 만들어 사용할 수 있다.
    - 각각의 컴포넌트 코드를 보다 간결하게 유지할 수 있다.

- 단점
    - 중재자 역할을 하는 클래스의 복잡도와 결합도가 증가한다.

ExecutorService, Executor, DispatcherServlet

### 메멘토 패턴
캡슐화를 유지하면서 객체 내부 상태를 외부에 저장하는 방법
- 객체 상태를 외부에 저장했다가 해당 상태로 다시 복구할 수 있다.

Serializable object -> byte -> object
일종의 메멘토일뿐

직렬화를 사용하려면 더 많은 공부가 필요함

### 옵저버 패톤
다수의 객체가 특정 객체 상태 변화를 감지하고 알림을 받는 패턴
- 발행-구독 패턴을 구현할 수 있다.

- 장점
    - 상태를 변경하는 객체와 변경을 감지하는 객체의 관계를 느슨하게 유지할 수 있다.
    - Subject의 상태 변경을 주기적으로 조회하지 않고 자동으로 감지할 수 있다.
    - 런타임에 옵저버를 추가하거나 제거할 수 있다.

- 단점
    - 복잡도가 증가한다.
    - 다수의 Observer 객체를 등록 이후 해지 않는다면 memory leak이 발생할 수도 있다.

### 상태(스테이트) 패턴
객체 내부 상태 변경에 따라 특정 오브젝트에 행동이 달라지는 패턴

- 상태에 특화된 행동들을 분리해 낼 수 있으며, 새로운 행동을 추가하더라도 다른 행동에 영향을 주지 않는다.

- 장점
    - 상태에 따른 동작을 개별 클래스로 옮겨서 관리할 수 있다.
    - 기존의 특정 상태에 따른 동작을 변경하지 않고 새로운 상태에 다른 동작을 추가할 수 있다.
    - 코드 복잡도를 줄일 수 있다.

- 단점
    - 복잡도가 증가한다.

### 전략 패턴
여러 알고리즘을 캡슐화하고 상호 교환 가능하게 만드는 패턴

- 컨텍스트에서 사용할 알고리즘을 클라이언트가 선택한다.

comparator

- 장점
    - 새로운 전략을 추가하더라도 기존 코드를 변경하지 않는다.
    - 상속 대신 위임을 사용할 수 있다.
    - 런타임에 전략을 변경할 수 있따.
- 단점
    - 복잡도가 증가한다.
    - 클라이언트 코드가 구체적인 전략을 알아야 한다.

### 템플릿 메소드 패턴
알고리즘 구조를 서브 클래스가 확장할 수 있도록 템플릿으로 제공하는 방법

- 추상 클래스는 템플릿을 제공하고 하위 클래스는 구체적인 알고리즘을 제공한다.

- 장점
    - 코드 중복을 줄일 수 있다.
    - 템플릿 코드를 변경하지 않고 상속을 받아서 구체적인 알고리즘만 변경할 수 있따.

- 단점
    - 리스코프 치환 원칙을 위반할 가능성이 있다.
    - 알고리즘 구조가 복잡할 수록 템플릿을 유지하기 어려워진다.

### 템플릿 콜백 패턴
콜백으로 상속 대신 위임을 사용하는 템플릿 패턴
- 상속 대신 익명 내부 클래스 또는 람다 표현식을 활용할 수 있다.


### 비지터 패턴
기존의 코드를 건드리지 않고 새로운 코드를 추가하는 것

- 더블 디스패치를 활용할 수 있다.









