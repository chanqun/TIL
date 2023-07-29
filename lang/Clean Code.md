# Clean Code

## Chapter 1 - Clean Code

### The Art of Clean Code?
클린코드란 예술작품과 같다. 어떤 코드가 클린코드 인지 아닌지를 구분하는 것과 클린코드를 쓸 수 있는지는 다른 문제이다.
클린코드를 작성하려면 피를 토해가며 얻은 클린코드에 대한 감각을 사용해 무수하게 많은 작은 기술들을 적용해야 한다.

### What is Clean Code?

Bjarne Stroustrup
- 코드는 즐겁게 읽혀야 한다.
- 효율적인 코드라야 한다. 이는 성능적 측면 뿐만 아니라 나쁜 코드는 난장판을 더 키우기 때문이다.(깨진 유리창 이론) 1
- 에러 핸들링, 메모리 누수, 경쟁상태, 일관되지 않은 네이밍 등 디테일을 신경쓰라.
- 나쁜 코드는 여러가지 일을 하려고 한다. 나쁜 코드는 애매한 의도와 모호한 목적을 포함한다. 클린코드는 한 가지에 집중한다. 클린코드는 한 가지 일을 잘 한다.

Grady Booch
- 클린코드는 하나의 잘 쓰여진 산문처럼 읽혀야 한다. 소설의 기승전결처럼 문제를 제시하고 명쾌한 해답을 제시해야 한다.
- 명백한 추상: 코드는 추측 대신 실제를 중시, 필요한 것만 포함하며 독자로 하여금 결단을 내렸다고 생각하게 해야 한다.

Dave Thomas
- 다른 이가 수정하기 쉬워야 한다.
- 테스트를 해야 한다.
- 코드는 간결할 수록 좋다.(Smaller is better)
- 코드는 세련되어야 한다.

Michael Feathers
- 코드를 care하라

Ron Jeffries

- 중복을 없애라
- 클래스/메서드는 한 가지 일만 하게 하라
- 메서드의 이름 등으로 코드가 하는 일을 명시하라
- (메서드 등을) 일찍 추상화해서 프로젝트를 빠르게 진행할 수 있게 하라

Ward Cunningham
- 읽고, 끄덕이고, 다음으로 넘어갈 수 있는 코드를 작성하라.
- 당신이 사용하는 언어를 탓하지 말라. 코드를 아름답게 만드는 것은 프로그래머이다.

### Prequel and Principles
이 책을 "클린코드에 대한 객체 멘토 학파"라고 생각하라.

## Chapter 2 - Meaningful Names

### Use Intention-Revealing Names
### Avoid Disinformation
- 중의적으로 해석될 수 있는 이름 지양하기.
- 개발자에게는 특수한 의미를 가지는 단어(List 등)는 실제 컨테이너가 List가 아닌 이상 accountList와 같이 변수명에 붙이지 말자. 차라리 accountGroup, bunchOfAccounts, accounts등으로 명명하자
- 비슷해 보이는 명명에 주의하자.

### Make Meaningful Distinctions
- 말이 안되는 단어(한 글자만 바꾼다던지 한 단어), [a1, a2, …]과 같이 숫자로 구분하는 경우 주의
- 클래스 이름에 Info, Data와 같은 불용어를 붙이지 말자. 정확한 개념 구분이 되지 않음

> Name VS NameString
> getActiveAccount() VS getActiveAccounts() VS getActiveAccountInfo() (이들이 혼재할 경우 서로의 역할을 정확히 구분하기 어렵다.)
> money VS moneyAmount
> message VS theMessage

### Use Pronounceable Names

### Use Searchable Names
- 상수는 static final과 같이 정의해 쓰자.
- 변수 이름의 길이는 변수의 범위에 비례해서 길어진다.

### Avoid Encodings

Hungarian Notation, Member Prefixes, Interfaces and Implementations

### Avoid Mental Mapping
### Don't Be Cute

### Pick One Word per Concept
- 추상적인 개념 하나에 단어 하나를 사용하자.
  - fetch, retrieve, get
  - controller, manager, driver

### Don't Pun (말장난)

### Use Solution Domain Names
So go ahead and use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth.
### Use Problem Domain Names
### Add Meaningful Context

### Don't Add Gratuitous Context

## Chapter 3 - Functions

### Small
The first rule of functions is that they should be small.

### Do One Thing

### One Level of Abstraction per Function
-> Reading Code from Top to Bottom : The Stepdown Rule

### Use Descriptive Names

### Have No Side Effects
Output Arguments :
In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.

### Command Query Separation

### Prefer Exceptions to Returning Error Codes

### Don't Repeat Yourself

### Structured Programming

Some programmers follow Edsger Dijkstra’s rules of structured programming.
Dijkstra said that every function, and every block within a function, should have one entry and one exit.

### How Do You Write Functions Like This?
처음에는 길고 복잡하고, 들여쓰기 단계나 중복된 루프도 많다.
인수목록도 길지만, 이 코드들을 빠짐없이 테스트하는 단위 테스트 케이스도 만들고, 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거한다. 처음부터 탁 짜지지는 않는다.

## Chapter 4 - Comments

