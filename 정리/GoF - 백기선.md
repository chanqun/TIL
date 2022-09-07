## 싱글톤 패턴

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


## 팩토리 메소드 패턴
구체적으로 어떤 인스턴스를 만들지는 서브 클래스가 정한다.
- 다양한 구현체가 있고, 그중에서 특정한 구현체를 만들 수 있는 다양한 팩토리를 제공할 수 있다.

OpenClose 원칙 확장에 대해서는 열려있고, 수정에 대해서는 닫혀있다.

- 단순한 팩토리 패턴
    - 매개변수의 값에 따라 또는 메소드에 따라 또는 메소드에 각기 다른 인스턴스를 리턴하는 단순한 버전의 팩토리 패턴
    - java.lang.Calendar 또는 java.lang.NumberFormat 클래스
- 스프링 BeanFactory
    - Object 타입의 Product를 만드는 BeanFactory라는 Creator