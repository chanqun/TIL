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
