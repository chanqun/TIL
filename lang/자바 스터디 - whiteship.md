
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
  - Thread(pc, jvm, stack, native method stack) : 프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다. 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장한다.
  - Heap(Eden, Survivor 0, Survivor 1, Old, Permanent)
    - Permanent : 생성된 객체들의 정보의 주소값이 저장된 공간이다. 클래스 로더에 의해 load 되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다.
      Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
  - Method Area(Runtime Constant Pool) : 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간, 상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.



### JDK, JRE
9부터는 jdk만 나옴
jdk에 jre 들어있음
