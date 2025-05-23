# JVM 밑바닥까지 파헤치기

## 1부 자바와 친해지기

### 1장 자바 기술 시스템 소개

#### 1.1 들어가며

자바의 특징
- 하드웨어 플랫폼이라는 족쇄를 제거하여 "한 번 작성하면 어디서든 실행된다"라는 이상을 실현
- 상당히 안전한 메모리 관리 시스템을 갖춘 덕에 메모리 누수 문제와 엉뚱한 메모리를 가리키는 문제 대부분을 피할 수 있다.
- 런타임에 핫 코드를 감지, 컴파일하고 최적화하여 자바 애플리케이션이 최상의 성능을 내도록 도와준다.
- 표준 API 자체가 풍부할 뿐 아니라 수많은 기업과 오픈 소스 커뮤니티에서 제공하는 다양한 기능의 서드 파티 라이브러리를 활용할 수 있다.


#### 1.2 자바 기술 시스템
- 자바 프로그래밍 언어, 자바 가상 머신 구현, 클래스 파일 포맷, 자바 클래스 라이브러리 API, 다른 기업과 오픈 소스 커뮤니테이서 제공하는 서드 파티 클래스 라이브러리
![자바](../image/자바기술시스템의구성요소.png)

#### 1.3 자바의 과거와 현재
#### 1.3.1 자바의 탄생
제임스 고슬링

#### 1.3.5 모던 자바의 시작
2014년: JDK 8
- JEP 126: 람다식 지원
- JEP 104: 자바스크립트 엔진 내장
- JEP 150: 새로운 시간 및 날짜 API
- JEP 122: 핫스팟에서 영구 세대 완전 제거

#### 1.4 자바 가상 머신 제품군
다양한 VM 들이 있었음

#### 1.5 자바 기술의 미래
그랄VM, 발할라, 앰버, 파나마 등
지속적으로 발전중 : var, switch, Pattern Matching, Records, Sealed Classes

#### 1.6 실전: 내 손으로 빌드하는 JDK

## 2부 자동 메모리 관리

### 2장 자바 메모리 영역과 메모리 오버플로

#### 2.1 들어가며
자바 개발자는 가상 머신이 제공하는 자동 메모리 관리 메커니즘 덕에 메모리 할당과 해제를 짝지어 코딩하지 않아도 메모리 누수나 오버플로 문제를 거의 겪지 않는다.
하지만 통제권을 위임했기 때문에 문제가 한번 터지면 가상 머신의 메모리 관리 방식을 이해하지 못하는 한 해결하기가 상당히 어렵다.

#### 2.2 런타임 데이터 영역
JVM은 자바 프로그램을 실행하는 동안 필요한 메모리를 몇 개의 데이터 영역으로 나눠 관리한다.
어떤 영역은 프로세스의 시작과 동시에 만들어지며, 어떤 영역은 사용자 스레드의 시작/종료에 맞춰 생성/삭제된다.

![img.png](../image/jvm데이터영역.png)

##### 2.2.1 프로그램 카운터 (pc)
**프로그램 카운터 레지스터**는 작은 메모리 영역으로, 현재 실행 중인 스레드의 '바이트코드 줄 번호 표시기'라고 생각
바이트코드 인터프리터는 이 카운터의 값을 바꿔 다음에 실행할 바이트코드 명령어를 선택하는 식으로 동작한다.
프로그램의 제어 흐름, 분기, 순환, 점프 등을 표현하는 것이다. 예외 처리나 스레드 복원 같은 모든 기본 기능이 바로 이 표시기를 활용해 이루어진다.

자바 가상 머신에서의 멀티스레딩은 CPU 코어를 여러 스레드가 교대로 사용하는 방식으로 구현되기 때문에 특정 시각에 각 코어는 한 스레드의 명령어만 실행하게 된다.
따라서 스레드 전환 후 이전에 실행하다 멈춘 지점을 정확하게 복원하려면 스레드 각각에는 고유한 프로그램 카운터가 필요하다. 따라서 각 스레드의 카운터는 서로 영향을 주지 않는 독립된 영역에 저장된다.
이 메모리 영역을 **스레드 프라이빗 메모**라고 한다.

스레드가 자바 메서드를 실행 중일 때는 실행 중인 바이트코드 명령어의 주소가 프로그램 카운터에 기록된다.
한편 스레드가 네이티브 메서드를 실행 중일 때 프로그램 카운터 값은 Undefined다. 프로그램 카운터 메모리 영역은 OutOfMemoryError조건이 명시되지 않는 유일한 영역

##### 2.2.2 자바 가상 머신 스택

프로그램 카운터처럼 자바 가상 머신 스택도 '스레드 프라이빗'하며, 연결된 스레드와 운명을 같이 한다.
각 메서드가 호출될 때마다 자바 가상 머신은 스택 프레임을 만들어 지역 변수 테이블, 피연산자 스택, 동적 링크, 메서드 반환값 등의 정보를 저장한다.
그런 다음 스택 프레임을 가상 머신 스택에 푸시하고, 메서드가 끝나면 팝하는 일을 반복한다.

자바의 메모리 영역을 힙 메모리와 스택 메모리로 구분하는 사람이 많다. c++ 메모리 구조에서 기인한 것이다.
스택이라고 하면 보통 지역 변수 테이블을 가리킬 때가 많다. 지역 변수 테이블에는 자바 가상 머신이 컴파일타임에 알 수 있는 다양한 기본 데이터 타입, 객체 참조, 반환 주소 타입을 저장한다.
지역 변수 테이블에서 이 데이터 타입들을 저장하는 공간을 지역 변수 슬롯이라 한다. 일반적으로 슬롯 하나의 크기는 32비트다.
double 타입은 64, 나머지 타입은 모두 슬롯 하나에 저장된다. 지역 변수 테이블을 구성하는데 필요한 데이터 공간은 컴파일 과정에서 할당된다.
자바 메서드는 스택 프레임에서 지역 변수용으로 할당받아야 할 공간의 크기가 이미 완벽하게 결정되어 있다.
메서드 실행 중에는 절대 변하지 않는다. 

스택 메모리 영역에서 두 가지 오류가 발생할 수 있는데
- 스레드가 요청한 스택 깊이가 가상 머신이 허용하는 깊이보다 크다면 StackOverflowError
- 스택 용량을 동적으로 확장할 수 있는 자바 가상 머신에서는 스택을 확장하려는 시점에 여유 메모리가 충분하지 않다면 OutOfMemoryError


##### 2.2.3 네이티브 메서드 스택

네이티브 메서드 스택은 가상 머신 스택과 매우 비슷한 역할을 한다. 차이점이라면 가상 머신 스택은 자바 메서드를 실행할때 사용하고,
네이티브 메서드 스택은 네이티브 메서드를 실행할 때 사용한다는 것이다. 네이티브메서드 스택에서 메서드를 어떤 구조로 어떻게
표현해야 하는지와 관련하여 아무것도 명시하지 않았다. 따라서 가상 머신 구현자가 원하는 형태로 자유롭게 표현할 수 있다.
가상 머신 스택처럼 네이티브 메서드 스택도 StackOverflowError, OutOfMemoryError 를 던질 수 있따.

##### 2.2.4 자바 힙

자바 힙은 자바 애플리케이션이 사용할 수 있는 가장 큰 메모리다. 자바 힙은 모든 스레드가 공유하며 가상 머신이 구동될 때 만들어진다. 이 메모리 영역의 유일한 목적은 객체 인스턴스를 저장하는 것이고,
자바 세계의 거의 모든 객체 인스턴스가 이 영역에 할당되나. <자바 가상 머신 명세>에는 '거의' 모든 객체 인스턴스와 배열은 힙에 할단된다라고 적혀있다.
JIT 컴파일 기술이 발전하면서, 스택 할당과 스칼라 치환 최적화 방식이 살짝 달라졌다.

자바 힙은 가비지 컬렉터가 관리하는 메모리 영역이기 때문에 어떤 문헌에서는 GC 힙이라고도 한다.
메모리 회수 관점에서 대다수 현대적인 GC는 세대별 컬렉션 이론을 기초로 설계됐다.
new generation, old generation, permanent generation, eden space, from survivor space, to survivor space
등을 반복해서 이야기할 것
G1 컬렉터가 등장한 2010년 전후로 핫스팟 가상 머신은 업계에서 확고한 주류가 되었음,
메모리 할당 관점에서 자바 힙은 모든 스레드가 공유한다. 따라서 객체 할당 효율을 높이고자 스레드 로컬 할당 버퍼 여러 개로 나뉜다. 하지만 자바 힙에 저장된다는 사실은 달라지지 않는다.
자바 힙을 작게 구분하는 목적은 오직 메모리 회수와 할당을 더 빠르게 하기 위함이다.

<자바 가상 머신 명세>에 따르면 자바 힙은 물리적으로 떨어진 메모리에 위치해도 상관없으나 논리적으로는 연속되어야 한다.
자바 힙은 크기를 고정할 수도, 확장할 수도 있게 구현할 수 있다. -Xmx, -Xms 새로운 인스턴스에 할당해 줄 힙 공간이 부족하고 힙을 더는 확장할 수 없다면
OutOfMemoryError 를 던진다.

##### 2.2.5 메서드 영역
메서드 영역도 자바 힙처럼 모든 스레드가 공유한다. 메서드 영역은 가상 머신이 읽어 들인 타입 정보, 상수, 정적 변수 그리고 JIT 컴파일러가 컴파일한 코드 캐시등을 저장하는 데 이용된다.
<자바 가상 머신 명세>에서는 메서드 영역도 논리적으로 힙의 한 부분으로 기술하지만 자바 힙과 구분하기 위해 논힙이라 부르기도 한다.

<자바 가상 머신 명세>는 메서드 영역에 제약을 거의 두지 않았다. 자바 힙과 마찬가지로 연속될 필요가 없으며, 크기를 고정할 수도 있고, 확장 가능하게 만들 수 있다.
또한 메서드 영역이 꽉 차서 필요한 만큼 메모리를 할당할 수 없다면 OutOfMemoryError 를 던진다.

##### 2.2.6 런타임 상수 풀

런타임 상수 풀은 메서드 영역의 일부다. 상수 풀 테이블에는 클래스 버전, 필드, 메서드, 인터페이스 등 클래스 파일에 포함된 설명 정보에 더해 컴파일타임에 생성된 다양한 리터럴과 심벌 참조가 저장된다.
가상 머신이 클래스를 로드할 때 이러한 정보를 메서드 영역의 런타임 상수 풀에 저장한다.

자바 가상 머신은 클래스 파일의 각 영역별로 엄격한 규칙을 정해 놓았다. 예컨대 가상 머신이 클래스 파일을 로드해 실행하려면 각 바이트에는 명세가 요구하는 데이터가 들어 있어야 한다.
다만 런타임 상수 풀에 대해서는 <자바 가상 머신 명세>가 요구 사항을 상세하게 정의하지 않아서 가상 머신 제공자가 입맛에 맞게 구현할 수 있다.

클래스 파일의 상수 풀과 비교해 런타임 상수 풀의 중요한 특징은 동적이라는 점이다. 자바 언어에서는 상수가 꼭 컴파일타임에 생성되어야 한다는 규칙이 없다.
즉 상수 풀의 내용 전부가 클래스 파일에 미리 완벽하게 기술되어 있는 게 아니가. 런타임에도 메서드 영역의 런타임 상수 풀에 새로운 상수가 추가될 수 있다.
String, intern() 메서드

런타임 상수 풀은 메서드 영역에 속하므로 자연스럽게 메서드 영역을 넘어서까지 확장될 수는 없다. 그래서 상수 풀의 공간이 부족하면 OutOfMemoryError를 던진다.

##### 2.2.7 다이렉트 메모리
다이렉트 메모리는 <자바 가상 머신 명세>에 정의된 영역도 아니지만 자주 쓰이는 메모리이며 OutOfMemoryError의 원인이 될 수 있다ㅏ.
JDK 1.4에서 NIO가 도입되면서 채널과 버퍼 기반 I/O 메서드가 소개됐다. NIO는 힙이 아닌 메모리를 직접 할당할 수 있는 네이티브 함수 라이브러리를 이용하며
이 메모리에 저장되어 있는 DirectByteBuffer 객체를 통해 작업을 수행할 수 있다.
따라서 자바 힙과 네이티브 힙 사이에서 데이터를 복사해 주고받이 않아도 돼서 일부 시나리오에서 성능을 크게 개선했다.
물리 메모리를 직접 할당하기 때문에 자바 힙 크기의 제약과는 무관하지만 이역시 메모리라는 사실에는 변함이 없다.
따라서 하부 기기의 총 메모리 용량과 프로세서가 다룰 수 있는 주소 공간을 넘어설 수는 없다. 사용되는 모든 메모리 영역의 합이 물리 메모리 한계를 넘어서면 동적 확장을 시도할 때 
OutOfMemoryError가 발생한다.

#### 2.3 핫스팟 가상 머신에서의 객체 들여다보기

##### 2.3.1 객체 생성
언어 수준에서 객체 생성은 보통 단순히 new 키워드를 쓰면 끝난다. 그렇다면 가상 머신 수준에서는 과연 어떤 과정을 거쳐 객체가 생성될까?
자바 가상 머신이 new 명령에 해당하는 바이트코드를 만나면 이 명령의 매개 변수가 상수 풀 안의 클래스를 가리키는 심벌 참조인지 확인한다.
그런 다음 이 심벌 참조가 뜻하는 클래스가 로딩, 해석, 초기화 되었는지 확인한다. 준비되지 않은 클래스라면 로딩부터
로딩이 완료된 클래스라면 새 객체를 담을 메모리를 할당한다. 객체용 메모리 공간 할당은 자바 힙에서 특정 크기의 메모리 블록을 잘라 주는 일
자바 힙이 규칙적이냐 아니냐에 따라 달라지며, 자바 힙이 규칙적이냐는 사용하는 가비지 컬렉터가 컴팩트를 할 수 있느냐에 달렸다.
따라서 시리얼과 파뉴처럼 모으기가 가능한 컬렉터를 사용하는 시스템이라면 단순하고 효율적인 포인터 밀치기 방식의 할당 알고리즘을 채택하고
이론상의 CMS처럼 스윕 알고리즘을 적용한 컬렉터를 쓰는 시스템은 더 복잡한 여유 목록 방식을 채택할 것이다.

가상 머신에서 객체 생성은 매우 빈번하고, 멀티스레딩 환경에서는 여유 메모리의 시작 포인터 위치를 수정하는 단순한 일도 스레드 안전하지 않기 떄문에
여러 스레드가 동시에 객체를 생성하려고 할 때 문제가 생길 수 있다.

해법은 두 가지
첫 번째는 메모리 할당을 동기화하는 방법 실제로 CAS 실패 시 재시도 방식의 가상 머신은 갱신을 원자적으로 수행한다.
두 번째는 스레드마다 다른 메모리 공간을 할당하는 방법 TLAB, 각 스레드는 로컬 버퍼에서 메모리를 할당 받아 사용하다가 버퍼가 부족해지면 그때 동기화를 해 새로운 버퍼를 할당 받음
-XX:+/-UseTLAB

메모리 할당을 받으면 가상 머신은 할당받은 공간을 0으로 초기화
다음으로는 각 객체에 필요한 설정을 해 줌 : 어느 클래스의 인스턴스, 클래스의 메타 정보 찾는 법, 객체의 해시코드, GC 나이
이런 정보가 각 객체의 객체 헤더에 저장


##### 2.3.2 객체의 메모리 아웃
핫스팟 가상 머신은 객체를 세 부분으로 나눠 힙에 저장
헤더 : 객체 자체의 런타임 데이터
인스턴스 데이터 : 객체가 실제로 담고 있는 정
길이 맞추기용 정렬 패딩 : 특별한 의미 없이 자리를 확보

##### 2.3.3 객체에 접근하기
대다수 객체는 다른 객체 여러 개를 조합해 만들어짐
<자바 가상 머신 명세>는 참조 타입을 단지 객체를 가리키는 참조라고만 정했다. 주로 핸들이나 다이렉트 포인터를 사용해 구현

#### 2.4 실전 : OutOfMemoeryError 예외
<자바 가상 머신 명세>에 따르면 프로그램 카운터 외에도 가상 머신 메모리의 여러 런타임 영역에서 OutOfMemoryError가 날 수 있음

메모리 누수라면 도구를 이용해 누수된 객체로부터 GC 루트까지의 참조 사슬을 살표보고 어느 GC루트와 연결되어 있기에 가비지 컬렉터가 회수하지 못했는지 확인
메모리 누수가 아니라면, 모든 객체가 다 살아 있어야 한다면 자바 가상 머신의 힙 매개 변수 설정 -Xmx -Xms와 컴퓨터의 가용 메모리를 비교하여 가상 머신에 메모리를 더 많이 할당할 수 있는지 알아본다.
그다음에는 코드에서 수명 주기가 너무 길거나 상태를 너무 오래 유지하는 객체는 없는지, 공간 낭비가 심한 데이터 구조를 쓰고 있지는 않은지 살표 프로그램이 런타임에 소비하는 메모리를 최소로 낮춘다.

##### 2.4.2 가상 머신 스택과 네이티브 메서드 스택 오버플로
스택 크기는 오직 -Xss 매개 변수로만 변경할 수 있다.
StackOverFlowError, OutOfMemoryError 확인

##### 2.4.3 메서드 영역과 런타임 상수 풀 오버플로
영구 세대의 크기는 
JDK 7 이하 -XX:PermSize, -XX:MaxPermSize 매개 변수로 조절 할 수 있고 이는 상수 풀 용량에도 간접덕으로 영향을 준다.
JDK 8 이상 -XX:MetaspaceSize, -XX:MaxMetaspaceSize, -XX: MinMetaspaceFreeRatio

```java
public class RuntimeConstantPoolOOM {
    public static void main(String[] args) {
        String str1= new StringBuilder("컴퓨터").append(" 소프트웨어").toString();
        System.out.println(strl.intern() == str1); 
    }
}
```
jdk 6 false 처음은 영구 세대의 문자열 상수 풀에 복사, StringBuilder로 생성한 문자열 객체의 인스턴스는 자바 힙에 존재
jdk 7 이상 true intern() 문자열 상수 자바 힙

JDK 8 부터는 영구 세대가 사라지고, 메타스페이스를 이용


##### 2.4.4 네이티브 다이렉트 메모리 오버플로

-XX:MaxDirectMemorySize 따로 설정하지 않았다면 기본적으로 -Xmx로 설정한 자바 힙의 최댓값과 같음
메모리 오버플로로 생성된 덤프 파일이 매우 작고 프로그램에서 DirectMemory를 직접 또는 간접적 (NIO) 사용했다면,
다이렉트 메모리에서 원인을 찾아야한다.

### 3장 가비지 컬렉터와 메모리 할당 전략

#### 3.1 들어가며
동적 메모리 할당과 가비지 컬렉션 기술을 처음 사용한 언어는 1960년대 MIT에서 개발된 리스프

가비지 컬렉션이 처리해야 하는 문제
- 어떤 메모리를 회수해야 하나?
- 언제 회수해야 할까?
- 어떻게 회수해야 할까?

자바에서는 프로그램 카운터, 가상 머신 스택, 네이티브 메서드 스택은 스레드와 함꼐 생성되고 소멸

반면 자바 힙과 메서드 영역은 불확실한 게 많다. 같은 인터페이스라 해도 구현한 클래스마다 요구하는 메모리 크기가 다를 수 있다.
하나의 메서드에서도 어떤 조건 분기를 실행하느냐에 따라 메모리 요구량이 달라질 수 있다. 그래서 이 메모리 영역들의 할당과 회수는 동적으로 이루어진다.

#### 3.2 대상이 죽었는가?
자바 세계에서는 거의 모든 객체 인스턴스가 힙에 저장.

##### 3.2.1 참조 카운팅 알고리즘
1. 객체를 가리키는 reference counter 추가, 참조하는 곳이 하나 늘어날 때마다 카운터 값을 1씩 증가
2. 참조하는 곳이 하나 사라질 때마다 카운터 값을 1씩 감소
3. 카운터 값이 0이 된 객체는 더는 사용될 수 없다.

자바는 참조 카운팅 쓰지 않음.
순환 참조 circular reference 문제

##### 3.2.2 도달 가능성 분석 알고리즘
자바, C#등 오늘날의 주류 프로그래밍 언어들은 모두 객체 생사 판단에 reachability analysis 알고리즘 사용

GC루트 객체들을 시작 노드 집합으로 씀, reference chain 도달 불가능한 객체는 더 이상 사용할 수 없는 게 확실하므로 회수 대상

자바에서 GC 루트로 이용할 수 있는 객체는 정해져 있음
- 가상 머신 스택에서 참조하는 객체
- 메서드 영역에서 클래스가 정적 필드로 참조하는 객체
- 메서드 영역에서 상수로 참조되는 객체
- 네이티브 메서드 스택에서 JNI가 참조하는 객체
- 자바 가상 머신 내부에서 쓰이는 참조 (NullPointerException, OutOfMemoeryError)
- 동기화 락으로 잠겨 있는 모든 객체
- 자바 가상 머신 내부 상황을 반영하는 JMXBean

##### 3.2.3 다시 참조 이야기로

JDK 1.2 전의 자바에서는 '참조'를 다음과 같이 매우 전통적인 의미로 정의
참조 타입 데이터에 저장된 값이 다른 메모리 조각의 시작 주소를 뜻한다면, 이 참조 데이터를 해당 메모리 조각이나 객체를 참조한다고 말한다.

JDK 1.2부터 참조개념
- 강한 참조
- 부드러운 참조 Soft
- 약한 참조 Weak
- 유령 참조 Phantom

##### 3.2.4 살았나 죽었나?
도달 가능성 분석 알고리즘이 '도달 불가능'으로 판단한 객체라도 두 번의 marking과정을 거쳐야 한다.

##### 3.2.5 메서드 영역 회수하기
<자바 가상 머신 명세>에 따르면 가비지 컬렉터가 메서드 영역을 반드시 청소해야 하는 것은 아니다.

메서드 영역의 가비지 컬렉션은 크게 두 가지를 회수한다. 더 이상 사용되지 않는 상수와 클래스다
리플렉션, 동적 프록시, CGLib와 같은 바이트코드 프레임워크를 많이 사용하는 경우나 JSP를 동적으로 생성하고 클래스 로더를 자주 사용자화하는 
OSGi 환경 등에서는 일반적으로 자바 가상 머신이 타입 언로딩을 지원해야 한다.

#### 3.3 가비지 컬렉션 알고리즘

##### 3.3.1 세대 단위 컬렉션 이론

1. 약한 세대 가설 : 대다수 객체는 일찍 죽는다.
2. 강한 세대 가설 : 가비지 컬렉션 과정에서 살아남은 횟수가 늘어날수록 더 오래 살 가능성이 커진다.

마크-스윕, 마크-카피, 마크-컴팩트 등의 가비지 컬렉션 알고리즘을 구분해 적용함

세대 단위 커렉션 이론을 가상 머신에 적용한 설계자들은 자바 힙을 최소 두 개 영역을 나눈다.

신세대에서만 가비지 컬렉션을 하고 싶더라도 신세대에 속하지만 구세대에서 참조 중인 객체도 충분히 있을 수 있음
이래서 나온 것이
3. 세대 간 참조 가설 : 세대 간 참조의 개수는 같은 세대 안에서의 참조보다 훨씬 적다.

마이너 GC가 수행 됨녀 세대 간 참조를 포함하는 작은 메모리 블록 안의 객체들만 GC루트에 추가 됨

##### 3.3.2 마크-스윕 알고리즘

mark and sweep

- 실행 효율이 일정하지 않음
- 메모리 파편화가 심함

##### 3.3.3 마크-카피 알고리즘
가용 메모리를 똑같은 크기의 두 블록으로 나눠 한 번에 한 블록만 사용
한쪽 블록이 꽉 차면 살아남은 객체들만 다른 블록에 복사하고 기존 블록을 청소
신세대에는 대부분 이 알고리즘을 사용

메모리 할당 보증 매커니즘 : 마이너 GC에서 살아남은 객체를 생존자 공간이 다 수용하지 못할 경우 다른 메모리 여영ㄱ을 활용해 메모리 할당을 보증하는 것

##### 3.3.4 마크-컴팩트 알고리즘
스탑 더 월드

메모리 읽기는 사용자 프로그램에서 가장 빈번하게 일어나는 과정, 위와 같은 관점에서 객체를 이동시킬 때와 아닐 때 모두 단점이 있음

대부분의 경우에는 메모리 파편화 감내하면서 마크-스윕하다가 객체 할당에 영향을 줄 만큼 파편화 심해지면 마크-컴팩트를 돌려 공간을 확보하기도 함
CMS

#### 3.4 핫스팟 알고리즘 상세 구현

##### 3.4.1 루트 노드 열거
루트 노드 열거는 일관성이 보장되는 스냅숏 상태에서 수행해야 한다.
일관성이란 열거 작업이 진행되는 동안 실행 서브시스템이 특정 시점으로 고정된 것처럼 보인다는 뜻

##### 3.4.2 안전 지점
핫스팟은 OopMap을 활용하여 GC 루트들을 빠르고 정확하게 열거할 수 있음
안전지점을 너무 적게 설정해서 컬렉터가 너무 오래 기다리게 하거나, 반대로 너무 많이 설정해서 런타임 메모리 부하가 지나치게 커지지 않도록 주의해야 한다.
안전 지점의 수를 적절히 조절하여 컬렉터의 대기 시간과 런타임 메모리 부하 사이의 균형을 맞춰야 한다.

선제적 멈춤, 자발적 멈춤

메서드 호출, 순환문, 예외 처리 등 명령어 흐름을 다중화하는 지점에 안전 지점을 생성

##### 3.4.3 안전 지역

안전 지점의 한계
실행 중이 아닌 스레드 처리, 무한 대기 상태

안전 지역 개념 도입, 참조 관계가 변하지 않는 코드 영역
영역 내 어디서든 가비지 컬렉션 시작 기능

스레드가 안전 지역 진입 시, 가비지 컬렉터는 안전 지역 내 스레드를 무시, 스레드가 안전 지역 이탈 시 GC 단계 완료 여부 확인,
GC 미완료 시 완료 신호까지 대기

##### 3.4.4 기억 집합과 카드 테이블 
Remembered Set, 비회수 영역에서 회수 영역을 가리키는 포인터들을 기록하는 추상 데이터 구조
구세대와 GC 루트 전체를 스캔하는 비효율을 방지

카드 테이블 : 카드 정밀도로 구현된 기억 집합
각 바이트(카드)가 특정 크기의 메모리 블록에 대응


##### 3.4.5 쓰기 장벽

카드 테이블을 효율적으로 관리하여 GC 루트 스캔 범위를 줄임
대입 연산 후 카드 테이블 갱신
거짓 공유 문제 

##### 3.4.6 동시 접근 가능성 분석
흰색: 가비지 컬렉터가 방문한 적 없는 객체
검은색: 가비지 컬렉터가 방문한 적이 있으며, 이 객체를 가리키는 모든 참조를 스캔 
회색: 가비지 컬렉터가 방문한 적이 있으며, 이 객체를 가리키는 모든 참조를 스캔을 완료하지 않음

#### 3.5 클래식 가비지 컬렉터
<자바 가상 머신 명세>는 가비지 컬렉터를 어떻게 구현해야 하는지 규정하지 않았다.

##### 3.5.1 시리얼 컬렉터
##### 3.5.2 파뉴 컬렉터
##### 3.5.3 패러렐 스캐빈지 컬렉터
##### 3.5.4 시리얼 올드 컬렉터
##### 3.5.5 패러렐 올드 컬렉터
##### 3.5.6 CMS 컬렉터
##### 3.5.7 G1 컬렉터
##### 3.5.8 오늘날의 가비지 컬렉터들

#### 3.6 저지연 가비지 컬렉터
##### 3.6.1 세넌도어
##### 3.6.2 ZGC
##### 3.6.3 세대 구분 ZGC

#### 3.7 적합한 가비지 컬렉터 선택하기

##### 3.7.1 엡실론 컬렉터
##### 3.7.2 컬렉터들 간 비교 및 취사선택
##### 3.7.3 가상 머신과 가비지 컬렉터 로그

#### 3.8 메모리 할당과 회수 전략
##### 3.8.1 객체는 먼저 에덴에 할당
##### 3.8.2 큰 객체는 곧바로 구세대에 할당
##### 3.8.3 나이가 차면 구세대로 옮겨짐
##### 3.8.4 공간이 비좁으면 강제로 승격

### 4 가상 머신 성능 모니터링과 문제 해결 도구
예외 스택, 가상 머신 운영 로그, 가비지 컬렉터 로그, 스레드 덤프 스냅숏, 힙 덤프 스냅숏

#### 4.2 기본적인 문제 해결 도구
##### 4.2.1 jps: 가상 머신 프로세스 상태 도구
##### 4.2.2 jstat: 가상 머신 통계 정보 모니터링 도구
##### 4.2.3 jinfo: 자바 설정 정보 도구
##### 4.2.4 jmap: 자바 메모리 매핑 도구
##### 4.2.5 jhat: 가상 머신 힙 덤프 스냅숏 분석 도구
##### 4.2.6 jstack: 자바 스택 추적 도구
##### 4.2.7 기본 도구 요약

#### 4.3 GUI 도구

##### 4.3.1 JHSDB: 서비스 에이전트 기반 디버깅 도구
##### 4.3.2 JConsole: 자바 모니터링 및 관리 콘솔
##### 4.3.3 VisualVM: 다용도 문제 대응 도구
##### 4.3.4 JMC: 지속 가능한 온라인 모니터링 도구

#### 4.4 핫스팟 가상 머신 플로그인과 도구

##### 4.4.1 HSDIS
##### 4.4.2 JITWatch


### 5 최적화 사례 분석 및 실정

#### 5.2 사례 분석
##### 5.2.1 대용량 메모리 기기 대상 배포 전략
##### 5.2.2 클러스터 간 동기화로 인한 메모리 오버플로
##### 5.2.3 힙 메모리 부족으로 인한 오버플로 오류
##### 5.2.4 시스템을 느려지게 하는 외부 명령어
##### 5.2.5 서버 가상 머신 프로세스 비정상 종료
##### 5.2.6 부적절한 데이터 구조로 인한 메모리 과소비
##### 5.2.7 윈도우 가상 메모리로 인한 긴 일시 정지
##### 5.2.8 안전 지점으로 인한 긴 일시 정지

##### 5.3.1 최적화 전 상태
##### 5.3.2 JDK 버전 업그레이드에 따른 성능 변화
##### 5.3.3 클래스 로딩 시간 최적화
##### 5.3.4 컴파일 시간 최적화
##### 5.3.5 메모리 설정 최적화
##### 5.3.6 적절한 컬렉터 선택으로 지연 시간 단축


## 3부 자동 메모리 관리

### 6장 클래스 파일 구조



#### 6.2 플랫폼 독립을 향한 초석

자바 탄생 시 내건 구호 한 번 작성하면 어디서든 실행된다.
자바 기술은 초기 설계부터 가상 머신에서 다른 언어를 실행할 가능성을 염두에 두었음. <자바 언어 명세>와 <자바 가상 머신 명세>를 나눠 놓음
<자바 가상 머신 명세>에서는 미래에는 다른 언어들을 더 잘 지원하도록 자바 가상 머신을 확장할 것이라고 명시함

javac 컴파일러, kotlinc 컴파일러 등 -> 바이트 코드(*.class) -> 자바 가상 머신


#### 6.3 클래스 파일의 구조

클래스 파일은 바이트를 하나의 단위로 하는 이진 스트림 집합체다.
각 데이터 항목이 정해진 순서에 맞게, 구분 기호 없이 조밀하게 나열된다.
그래서 클래스 파일 전체가 낭비되는 공간 없이 프로그램을 실행하는 데 꼭 필요한 데이터로 체워진다. <자바 가상 머신 명세>에 따르면 클래스 파일에 데이터를 저장하는 데는 c 언어의 구조체와 비슷한 의사 구조를 이용
- 부호 없는 숫자 : u1, u2 바이트
- 테이블: 여러 개의 부호 없는 숫자나 또 다른 테이블로 구성된 복합 데이터 타입을 표현


##### 6.3.1 매직 넘버와 클래스 파일의 버전

모든 클래스 파일의 처음 4바이트는 매직 넘버로 시작한다. 매직 넘버는 가상 머신이 허용하는 클래스 파일인지 여부를 빠르게 확인하는 용도로만 쓰인다.
클래스 파일의 매직 넘버 0xCAFEBABE


##### 6.3.2 상수 풀
상수 풀은 클래스 파일의 자원 창고, 클래스 파일 구조에서 다른 클래스와 가장 많이 연관된 부분

##### 6.3.3 접근 플래그
상수 풀 다음의 2바이트는 현재 클래스의 접근 정보를 식별하는 접근 플래그
class, interface, public, abstract final 인지

##### 6.3.4 클래스 인덱스, 부모 클래스 인덱스, 인터페이스 인덱스
현재 클래스 인덱스 (this_class)와 부모 클래스 인덱스 (super_class), 인터 페이스 인덱스 컬렉션(interface)이 나옴, 이러한 정보는 클래스 파일의 상속 관계를 규정한다.

##### 6.3.5 필드 테이블
필드 테이블(field_info)은 인터페이스나 클래스 안에 선언된 변수들을 설명하는데 쓰임



##### 6.3.6 메서드 테이블
method_info
클래스 파일에서 메서드 저장 형태는 필드 저장 형태와 거의 같다

##### 6.3.7 속성 테이블
속성 테이블 (attribute info)는 클래스 파일, 필드 테이블, 메서드 테이블, Code 속성, 레코드 구성 요소는 모두 특정 시나리오에서 특정한 정보를 설명하기 위해 고유한 속성 테이블을 포함할 수 있다.

#### 6.4 바이트코드 명령어 소개
바이트코드 명령어 집합은 고유한 특징과 장단점이 있는 명렁어 집합 아키텍처

##### 6.4.1 바이트코드와 데이터 타입들

##### 6.4.2 로드와 스토어 명령어
로드와 스토어 명령어는 스택 프레임의 지역 변수 테이블과 피연산자 스택 사이에서 데이터를 주고받는데 쓰인다.

##### 6.4.3 산술 명령어
산술 명령어들은 피연산자 스택의 값 두 개를 이용해 특정한 산술 연산을 수행하고, 결과값을 다시 피연산자 스택의 맨 위에 저장한다.

##### 6.4.4 형 변환 명령어
형 변환 명령어는 숫자 타입 데이터를 다른 숫자 타입으로 변환

##### 6.4.5 객체 생성과 접근 명령어
클래스 인스턴스와 배열도 객체

##### 6.4.6 피연산자 스택 관리 명령어
피연산자 스택을 직접 조작하는 명령어들

##### 6.4.7 제어 전이 명령어
프로그램의 실행 흐름을 조건에 따라 또는 무조건적으로 저장한 위치의 명령어로 이동시킨다.

##### 6.4.8 메서드 호출과 변환 명령어
- Invoke
    - virtural, interface, special, static, dynamic

##### 6.4.9 예외 처리 명령어
자바 프로그램에서 throw 문으로 예외를 명시적으로 던지는 작업은 athrow명령어로 구현된다.

##### 6.4.10 동기화 명령어
자바 가상 머신은 메서드 수준 동기화와 메서드 안의 명령어 블록 동기화를 지원한다.
두 구조의 동기화 모두 구현에는 모니터(락)을 이용한다.


#### 6.5 설계는 공개, 구현은 미공개
<자바 가상 머신 명세>는 공통된 프로그램 저장 형식을 정의하고 있다. 이는 모든 가상 머신이 지켜야 하는 약속이며, 어떤 하드웨어나 운영 체제를 사용하든 상관없어여 한다. 하지만 어떻게 구현 했는지는 공개 되지 않을 수 있다.

#### 6.6 클래스 파일 구조의 진화
<자바 가상 머신 명세>의 초판이 쓰인 지 20년도 지났음.
특정 하드웨어나 운영 체제에 종속되지 않는 플랫폼 독립성, 클래스 파일 형식의 간결성, 안전성, 확장성은 자바 기술 시스템이 플랫폼 독립과 언어 독립이라는 두 마리 토끼를 모두 잡기 위한 중요한 두 기둥이다.


### 7장 클래스 로딩 메커니즘

#### 7.1 들어가며
자바 가상 머신은 클래스를 설명하는 데이터를 클래스 파일로부터 메모리로 읽어 들이고 그 데이터를 검증, 변환, 초기화하고 나서 최종적으로 가상 머신이 곧바로 사용할 수 있는 자바 타입을 생성
컴파일 시 링크까지 해야 하는 언어들과 달리 자바 언어에서는 클래스 로딩, 링킹, 초기화가 모두 프래그램 실행 중에 이루어짐
그래서 자바 언어는 AOT 컴파일에 제약이 생기고 클래스 로딩을 거치느라 실행 성능이 살짝 떨어짐

자바가 동적 확장 언어 기능을 제공할 수 있는 것은 런타임에 이루어지는 동적 로딩과 동적 링킹 덕분

#### 7.2 클래스 로딩 시점

로딩 > [검증 > 준비 > 해석] 링킹 > 초기화 > 사용 > 언로딩
<자바 가상 머신 명세>는 클래스 로딩 과정의 첫 단계인 로딩을 정확히 어떤 상황에서 시작해야 하는지 명시하지 않았음

#### 7.3 클래스 로딩 처리 과정
##### 7.3.1 로딩
<자바 가상 머신 명세>로 정의되지 않아 다양한 방법이 있음
- ZIP, 네트워크, 런타임 동적 생성, 다른 파일로부터 생성, 데이터베이스 로딩, 암호화된 파일로부터 로딩

##### 7.3.2 검증
1. 클래스 파일의 바이트 스트림에 담긴 정보가 <자바 가상 머신 명세>에서 규정한 모든 제약을 만족하는지 확인한다.
2. 이 정보를 코드로 변환해 실행했을 때 자바 가상 머신 자체의 보안을 위협하지 않는지 확인한다.

##### 7.3.3 준비
준비는 클래스 변수를 메모리에 할당하고 초깃값을 설정하는 단계다.

##### 7.3.4 해석
해석은 자바 가상 머신이 상수 풀의 심벌 참조를 직접 참조로 대체하는 과정이다.

##### 7.3.5 초기화
초기화는 클래스 로딩의 마지막 단계다. 사용자 정의 클래스 로더를 이용하여 앞서 소개한 단계 중 일부에 관여할 수 있지만, 대부분의 작업은 온전히 자바 가상 머신이 통제한다.

#### 7.4 클래스 로더
클래스 로딩 단계 중 '완전한 이름을 보고 해당 클래스를 정의하는 바이너리 바이트 스트림 가져오기'를 가상 머신 외부에서 수행하도록 했다.

##### 7.4.1 클래스와 클래스 로더
각 클래스 로더는 독립적인 클래스 이름 공간을 지니기 때문에 클래스 로더를 빼놓고는 특정 클래스가 자바 가상 머신에서 유일한지 판단할 수 없다.
어떤 두 클래스가 동치인가 여부는 두 클래스 모두 같은 클래스 로더로 로드했을 때만 의미가 있다.

#### 7.5 자바 모듈 시스템
- requires, exports, open, uses, provides

### 8 바이트코드 실행 엔진
#### 8.1 들어가며
실행 엔진은 자바 가상 머신의 핵심 구성 요소. 가상 머신은 물리 머신에 대한 상대적인 개념
두 머신 모두 코드를 실행하는 기능을 한다.
<자바 가상 머신 명세>는 바이트 코드 실행 엔진의 개념 모델을 정의하고 있고, 이 모델이 주요 업체의 자바 가상 머신 실행 엔진들에 통일성을 심어 주는 핵심

#### 8.2 런타임 스택 프레임 구조
자바 가상 머신은 메서드를 가장 기본적인 실행 단위로 사용하며, 메서드 호출과 실행을 뒷받침하는 내부 데이터 구조로 스택 프레임을 이용

지역 변수 테이블 : 지역 변수 테이블은 메서드 매개 변수와 메서드 안에서 정의된 지역 변수를 저장하는 공간
피연산자 스택 : 피연산자 스택은 후입선출 스택
동적 링크 : 메서드에서 이용하는 외부 객체를 가리키는 참조는 런타임 상수 풀에 담겨 있으며, 각 메서드의 스택 프레임에서 런타임 상수 풀 내의 원소를 참조하는 식으로 구성된다. 이 참조가 동적 링크를 가능하게 하는 매개다.
반환 주소 : 시작된 메서드를 종료하는 방법은 실행 엔진이 반환 바이트코드 명령어를 만나면 메서드를 종료, 메서드 실행 도중 예외가 발생하고 메서드 본문에서 예외 처리가 제대로 이루어지지 않으면 종료
기타 정보 : <자바 가상 머신 명세>는 가상 머신이 스택 프레임에 추가 정보를 포함시킬 길을 열어 둠

#### 8.3 메서드 호출
메서드 호출 단계에서 수행하는 유일한 일은 호출할 메서드의 버전을 선택하는 것

##### 8.3.1 해석
##### 8.3.2 디스패치
- 정적 디스패치 : <자바 언어 명세> 에서는 원래 메서드 오버로드 해석이라고 표현한다.
- 동적 디스패치

- 단일 디스패치와 다중 디스패치 : 단일 디스패치는 한 볼륨 안에서 대상 메서드를 선택, 다중 디스패치는 둘 이상의 볼륨 안에서 대상을 찾음

#### 8.4 동적 타입 언어 지원
##### 8.4.1 동적 타임 언어
##### 8.4.2 자바와 동적 타이핑
##### 8.4.3 java.lang.invoke
심벌 참조만으로 호출 대상 메서드를 결정

#### 8.5 스택 기반 바이트코드 해석 및 실행 엔진
##### 8.5.1 해석 실행
##### 8.5.2 스택 기반 명령어 집합과 레지스터 기반 명령어 집합
##### 8.5.3 스택 기반의 해석 실행 과정


### 9 클래스 로딩과 실행 서브시스템, 사례와 실전
#### 9.2 사례 연구
클래스 로더와 바이트 코드 관련 사례, 일상 사례

##### 9.2.1 톰캣: 정통 클래스 로더 아키텍처
lib, common 패키지 구조
##### 9.2.2 OSGi: 유연한 클래스 로더 아키텍처
IBM 썬등이 참여한 동적 모듈 명세
번들들이 어떤 패키지를 공개하고 의존하느냐에 따라 번들 사잉의 위임과 의존 관계가 만들어짐

##### 9.2.3 바이트코드 생성 기술과 동적 프록시 구현
##### 9.2.4 백포트 도구: 자바의 타임머신

#### 9.3 실전: 원격 실행 기능 직접 구현하기
##### 9.3.1 목표
서버에서 임시 코드를 실행
- 컴파일된 자바 코드 실행하기
- 실행 결과 수집하기
- 구현

## 4부 컴파일과 최적화

### 10 프론트엔드 컴파일과 최적화

#### 10.1 들어가며
런타임에는 실행 효율을 높이는 최적화를 JIT 컴파일러가 지속해서 수행하고, 컴파일타임에는 개발자의 코딩 효율을 높이는 최적화를 프런트엔드 컴파일러가 수행하고 생각하면 된다.

#### 10.2 javac 컴파일러
##### 10.2.1 javac 소스 코드와 디버깅
##### 10.2.2 구문 분석과 심벌 테이블 채우기
##### 10.2.3 애너테이션 처리
##### 10.2.4 의미 분석과 바이트코드 생성

#### 10.3 자바 편의 문법의 재미난 점
##### 10.3.1 제네릭
##### 10.3.2 오토박싱, 오토언박싱, 개선된 for 문
##### 10.3.3 조건부 컴파일

#### 10.4 실전: 플러그인 애너테이션 처리기 제작


### 11장 백엔드 컴파일과 최적화

#### 11.1 들어가며
<자바 가상 머신 명세>는 어떤 컴파이러를 제공해야 한다고 규정하지 않음. 하지만 백엔드 컴파일러의 컴파일 성능과 최적화 품질은 상용 가상 머신의 우수성을 측정하는 핵심 지표

#### 11.2 JIT 컴파일러
자주 실행되는 메서드나 코드 블록이 발견되면 해당 코드를 네이티브 코드로 컴파일하고 다양한 최적화를 적용해 실행 효율을 높임
이러한 코드 블록을 핫스팟 코드 또는 핫 코드라고 함,

##### 11.2.1 인터프리터와 컴파일러
프로그램을 빠르게 시작하거나 메모리가 적을때는 인터프리터 방식이 좋음

인터 프리터 -> JIT 컴파일 -> 클라이언트 컴파일러 (C1), 서버 컴파일러 (C2)
          <- 최적화 취소 <-


##### 11.2.2 컴파일 대상과 촉발 조건
1. 여러 번 호출되는 메서드
2. 여러 번 실행되는 순환문의 본문

- 시간단 호출 횟수를 계산, 단위 시간 호출 횟수가 문턱값에 도달하지 못하면 카운터를 절반으로 줄임
- 클라이언트 모드 : 메서드 호출 카운터 문턱값 (13995) * OSR 비율 (default 933) / 100  
- 서버 모드 : (메서드 호출 카운터 문턱값(10700) * OSR 비율 (140) - 인터프리터 모니터링 비율(33)) / 100

JIT 컴파일 결과 확인 -XX:+PrintCompilation

#### 11.3 AOT 컴파일러
JIT 컴파일러는 애플리케이션이 실행되는 동안 컴파일러도 뒤에서 컴퓨팅 자원을 소비

AOT 장점
- 프로그램 실행 전 프로그램 코드를 네이티브 코드로 컴파일
  - AOT의 전통적인 형태이다.
  - 이를 위한 정보를 정확히 얻으려면 전체 프로그램을 대상으로 시간이 오래 걸리는 계산을 수행해야 한다.
  - 그랄 VM의 서브스트레이트 VM에서 이러한 형태의 최적화를 통해 네이티브 이미지를 생성한다.
- JIT 컴파일러가 런타임에 수행해야 하는 작업을 미리 수행해 캐시에 저장해 두고 다음번 실행 시 사용하는 형태
  - 동적 AOT / JIT 캐싱이라고 한다.

JIT의 장점
- 성능 기반 모니터링 최적화
  - 정적 분석 단계에서 얻을 수 없는, 실행 중 성능 모니터링 지표를 수집해 이를 바탕으로 최적화가 가능하다.
  - 특정 경로로의 실행이 자주 일어난다면 이 부분을 핫 코드로 지정하고, 더 많은 자원을 배분해 최적화가 가능하다.
- 급진적 예측 최적화
  - 성능 모니터링 정보를 토대로, 높은 확률로 정확한 판단이 가능하다. 설령 낮은 확률의 동작이 실행된다 하더라도 하위 계층 컴파일러 혹은 인터프리터로 수행하도록 하며, 오류가 나거나 잘못된 결과가 나오지는 않는다.
  - 가상 메서드를 인라인한다.
- 링크 타임 최적화
- Java는 본질적으로 동적 링크이며, 클래스가 런타임에 가상 머신에 로드된 후 JIT 에 의해 최적화된 네이티브 코드를 생성한다.
- AOT에서는 특정 동적 링크 라이브러리의 특정 메서드를 호출하려는 경우 메인 코드와 동적 링크 라이브러리 각각 독립적인 컴파일이 일어나, 최적화가 쉽지 않다.

#### 11.4 컴파일러 최적화 기법
- 메서드 인라인
- 탈출 분석 : 객체가 스레드에서 탈출하지 않는다고 확신하면 힙이 아니라 스택에 할당
- 공통 하위 표현식 제거
- 배열 경계 검사 제거 
- 오토박싱 제거, 안전지점 제거, 리플렉션 제거 등

그랄 컴파일러 : JIT 컴파일과 AOT 컴파일을 모두 지원

## 5부 효율적인 동시성

### 12장 자바 메모리 모델과 스레드

#### 12.1 들어가며
멀티태스킹이 중요해진 이유는, 컴퓨터의 연산 성능과 저장/통신 성능간 격차가 커졌기 때문
프로세서가 요청한 자원을 기다리며 놀고 있는 시간이 많아졌기 때문, 이런 문제를 해결하기 위해 멀티태스킹이 등장

#### 12.2 하드웨어에서의 효율과 일관성
공유 메모리 멀티프로세서 시스템, 비순차 실행 최적화

#### 12.3 자바 메모리 모델
<자바 가상 머신 명세>는 다양한 하드웨어와 운영 체제의 서로 다른 메모리 모델로부터 자바 프로그램을 보호하고자 따로 자바 메모리 모델을 정의

##### 12.3.1 메인 메모리와 작업 메모리

##### 12.3.2 메모리 간 상호 작용
- 잠금, 잠금 해제, 읽기, 적재, 사용, 할당, 저장, 쓰기

##### 12.3.3 volatile 변수용 특별 규칙
"volatile 변수는 모든 스레드에서 즉시 볼 수 있으며 , volatile 변수에 가해지는
모든 쓰기는 즉시 다른 스레드들에 반영된다. 다시 말해 volatile 변수의 값은 모
든 스레드에서 일관되므로 volatile 변수를 사용한 작업은 동시성 환경에서 안전
하다"

##### 12.3.4 long과 double 변수용 특별 규칙
long과 double에는 좀 더 느슨한 특별 규칙이 적용 32비트씩 비원자적 접근이 있을 수 있음.

##### 12.3.5 원자성, 가시성, 실행 순서

##### 12.3.6 선 발생 원칙
- 프로그램 순서 규칙 : 한 스레드 안에서는 제어 흐름 순서에 따라 앞의 연사이 뒤따르는 연산보다 선 발생
- 모니터 락 츄기 : 잠금 해제 연산은 같은 락에 대한 잠금 연산보다 선 발생
- 휘발성 변수 규칙
- 스레스 시작 규칙
- 스레드 종료 규칙
- 스레드 인터럽트 규칙
- 종료자 규칙
- 전이성

```java
private int value = 0;
public void setValue(int value) {
    this.value = value;
}
public int getValue() {
    return value;
}
```

- 두 메서드는 서로 다른 스레드에 의해 호출되므로 프로그램 순서 규칙은 여기에 적용되지 않는다.
- 동기화 블록이 없기 때문에 잠금 및 잠금 해제 연산이 수행되지 않으므로 모니터락 규칙도 적용되지 않는다.
- value 변수는 volatile이 아니므로 휘발성 변수 규칙도 적용되지 않는다.
- 이어지는 스레드 시작, 종료, 인터럽트 규칙과 종료자 규칙 역시 지금 문제와는 전혀 관련이 없다.
- 적용 가능한 선 발생 규칙이 없기 때문에 마지막 전이성도 문제가 되지 않는다.

멀티스레드 환경에서 안전하지 않음

##### 12.4.1 스레드 구현
커널 스레드, 사용자 스레드, 하이브리드 

##### 12.4.2 자바 스레드 스케줄링
협력적 스케줄링, 선점형 스케줄링

##### 12.4.3 상태 전이
신규, 실행 중, 무기한 대기, 시간 제한 대기, 블록, 종료

#### 12.5 자바와 가상 스레드
코루틴, 파이버


### 13장 스레드 안정성과 락 최적화

#### 13.1 들어가며
#### 13.2 스레드 안정성
여러 스레드가 한 객체에 동시에 접근할 때, 어떤 런타임 환경에서든 다음 두 조건을 모두 충족한면서 객체를 호출하는 행위가 올바른 결과를 얻을 수 있다면
그 객체는 스레드 안전하다라고 말한다.

- 특별한 스레드 스케줄링이나 대체 실행 수단을 고려할 필요 없다.
- 추가적인 동기화 수단이나 호출자 측에서 조율이 필요 없다.

안전한 정도
- 불변, 절대적 스레드 안전, 조건부 스레드 안전, 스레드 호환, 스레드 적대적

##### 13.2.2 스레드 안전성 구현
상호 배제 동기화
대기 중 인터럽트, 페어 락, 둘 이상의 조건 지정

- TAS, FAA, Swap, CAS, LL/SC

#### 13.3 락 최적화
##### 13.3.1 스핀 락과 적응형 스핀
적응형은 스핀락하면서 이전 스핀 시간과 락 소유자의 상태에 따라 결정된다는 뜻

##### 13.3.2 락 제거
락 제거는 데이터 경합이 없다고 판단하는 경우 JIT 컴파일러가 락을 제거하는 것

##### 13.3.3 락 범위 확장
연이은 다수의 작업이나 순환문에서 같은 락 객체를 반복해 사용하는 경우, 락 범위를 확장하여 락을 한 번만 획득

##### 13.3.4 경량 락
경량 락은 락을 획득하는 데 드는 비용을 줄이기 위해 사용되는 락으로, 스레드 경합을 없애 뮤텍스를 사용하는 기존 락의 성능을 향상시키기 위해 사용

##### 13.3.5 편향 락
편향 락은 경합이 없을 때 데이터의 동기화 장치들을 제거하여 프로그램 실행 성능을 높이는 최적화 기법
