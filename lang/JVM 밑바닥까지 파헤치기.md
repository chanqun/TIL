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

스레드가 자바 메서드를 실행 중일 떄는 실행 중인 바이트코드 명령어의 주소가 프로그램 카운터에 기록된다.
한편 스레드가 네이티브 메서드를 실행 중일 떄 프로그램 카운터 값은 Undefined다. 프로그램 카운터 메모리 영역은 OutOfMemoryError조건이 명시되지 않는 유일한 영역

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


