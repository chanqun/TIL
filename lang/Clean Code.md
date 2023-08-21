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

Therefore, the indent level of a function should not be greater than one or two.
This, of course, makes the functions easier to read and understand.

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

### Comments Do Not Make Up for Bad Code
### Explain Yourself in Code

```java
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)){}

if (employee.isEligibleForFullBenefits()){}
```

### Good Comments
- Legal Comments
- Informative Comments
- Explanation of Intent
- Clarification
- Warning of Consequences
- TODO Comments
- Amplification
- Javadocs in public APIs

### Bad Comments
- Mumbling
- Redundant Comments
- Misleading Comments
- Mandated Comments
- Journal Comments
- Noise Comments
- Scary Noise
- Don't Use a Comment When You Can Use a Function or a Variable
- Position Makers
- Closing Brace Comments
- Commented-out Code
- HTML Comments
- Nonlocal Information
- Too Much Information
- Inobvious Connection
- Function Headers
- Javadocs in Nonpublic Code


## Chapter 5 - Formatting

### The Newspaper Metaphor

### Vertical Formatting
#### Vertical Openness Between Concepts
If openness separates concepts, then vertical density implies close association.

### Horizontal Formatting
#### Horizontal Openness and Density


### Team Rules
Every programmer has his own favorite formatting rules, but if he works in a team, then the team rules.

## Chapter 6 - Objects and Data Structures

### Data Abstraction
A class does not simply push its variables out through getters and setters. 
Rather it exposes abstract interfaces that allow its users to manipulate the essence of the data, without having to know its implementation.

### Data/Object Anti-Symmetry
Objects hide their data behind abstractions and expose functions that operate on that data. 
Data structure expose their data and have no meaningful functions.

### The Law of Demeter
There is a well-known heuristic called the Law of Demeter2 that says a module should not know about the innards of the objects it manipulates.

### Data Transfer Objects

### Conclusion
Objects expose behavior and hide data. 
This makes it easy to add new kinds of objects without changing existing behaviors. 
It also makes it hard to add new behaviors to existing objects. 
Data structures expose data and have no significant behavior. 
This makes it easy to add new behaviors to existing data structures but makes it hard to add new data structures to existing functions.


## Chapter 7 - Error Handling

### Use Exceptions Rather Than Return Codes

### Write Your Try-Catch-Finally Statement First

### Use Unchecked Exceptions
checked exception은 상위 레벨 메소드에서 하위 레벨 메소드의 디테일에 대해 알아야 하기 때문에 캡슐화 또한 깨진다.

### Provide Context with Exceptions

### Define Exceptions Classes in Terms of a Caller's Need

### Define the Normal Flow

### Don't Return Null


### Don't Pass Null
Returning null from methods is bad, but passing null into methods is worse. 
Unless you are working with an API which expects you to pass null, you should avoid passing null in your code whenever possible.


## Chapter 8 - Boundaries

### Using Third-party Code
There is a natural tension between the provider of an interface and the user of an interface.
Providers of third-party packages and frameworks strive for broad applicability so they can work in many environments and appeal to a wide audience. 
Users, on the other hand, want an interface that is focused on their particular needs. 
This tension can cause problems at the boundaries of our systems.


### Exploring and Learning Boundaries

### Learning Tests Are Better Than Free

### Clean Boundaries

Good software designs accommodate change without huge investments and rework. 
When we use code that is out of our control, special care must be taken to protect our investment and make sure future change is not too costly.

Code at the boundaries needs clear separation and tests that define expectations.

We manage third-party boundaries by having very few places in the code that refer to them. We may wrap them as we did with Map, or we may use an ADAPTER to convert from our perfect interface to the provided interface.


## Chapter 9 - Unit Tests

### The Three Laws of TDD

First Law : You may not write production code until you have written a failing unit test
Second Law : You may not write more of a unit test than is sufficient to fail, and not compiling is failing
Third Law : You may not write more production code than is sufficient to pass the currently failing test

### Keeping Tests Clean

### Clean Tests
What makes a clean test? Three things. Readability, readability, and readability.
The same thing that makes all code readable: clarity, simplicity, and density of expression.


### One Accept per Test

### Single Concept per Test


### F.I.R.S.T
Clean tests follow five other rules that form the above acronym

- Fast
- Independent
- Repeatable
- Self-Validating
- Timely


## Chapter 10 - Classes

### Class Should Be Small!
With functions we measured size by counting physical lines. 
With classes we use a different measure. We count responsibilities.

### The Single Responsibility Principal
The Single Responsibility Principle (SRP)2 states that a class or module should have one, and only one, reason to change.

### Cohesion
You should try to separate the variables and methods into two or
more classes such that the new classes are more cohesive.

### Maintaining Cohesion Results in Many Small Classes

### Organizing for Change
Classes should be open for extension but closed for modification.

### Isolating from Change
If a system is decoupled enough to be tested in this way, it will also be more flexible and promote more reuse. 
The lack of coupling means that the elements of our system are better isolated from each other and from change. 
This isolation makes it easier to understand each element of the system.
By minimizing coupling in this way, our classes adhere to another class design principle known as the Dependency Inversion Principle (DIP).
In essence, the DIP says that our classes should depend upon abstractions, not on concrete details.

## Chapter 11 - Systems

### Separate Constructing a System from Using it
We should modularize this process separately from the normal runtime logic and we should make sure that we have a global, 
consistent strategy for resolving our major dependencies.

### Separation of Main
Factories, Dependency Injection

### Scaling Up

### Cross-Cutting Concerns
Java Proxies, Pure Java AOP Framework, AspectJ

### Test Drive the System Architecture

### Optimize Decision Making

### Use Standards Wisely, When They Add Demonstrable Value
### System Need Domain-Specific Languages

## Chapter 12 - Emergence

### Getting Clean via Emergent Design
Kent Beck’s four rules of Simple Design are of significant help in creating well-designed software.

- Runs all the tests
- Contains no duplication
- Expresses the intent of the programmer
- Minimizes the number of classes and methods


### Simple Design Rule 1 : Runs All the Tests

### Simple Design Rules 2-4 : Refactoring

### No Duplication
"reuse in the small" can cause system complexity to shrink dramatically.

### Expressive
- You can express yourself by choosing good names. 
- You can also express yourself by keeping your functions and classes small.
- You can also express yourself by using standard nomenclature.
- Well-written unit tests are also expressive.
- But the most important way to be expressive is to try.

### Minimal Classes and Methods
SRP can be taken too far. In an effort to make our classes and methods small, we might create too many tiny classes and methods.

### Conclusion
Following the practice of simple design can and does encourage and enable developers to adhere to good principles and patterns that otherwise take years to learn.

## Chapter 13 - Concurrency

### Why Concurrency?

### Myths and Misconceptions

- Concurrency always improves performance.
- Design does not change when writing concurrent programs.
- Understanding concurrency issues is not important when working with a container such as a Web or EJB container.
- Concurrency incurs some overhead, both in performance as well as writing additional code.
- Correct concurrency is complex, even for simple problems.
- Concurrency bugs aren’t usually repeatable, so they are often ignored as one-offs2 instead of the true defects they are.
- Concurrency often requires a fundamental change in design strategy.

### Concurrency Defense Principles

#### Single Responsibility Principle
- You will forget to protect one or more of those places—effectively breaking all code that modifies that shared data.
- There will be duplication of effort required to make sure everything is effectively guarded (violation of DRY7).
- It will be difficult to determine the source of failures, which are already hard enough to find.

Recommendation : Take data encapsulation to heart: severely limit the access of any data that may be shared

#### Corollary: Use Copies of Data
#### Corollary: Threads Should Be as Independent as Possible

#### Know Your Library

- Use the provided thread-safe collections.
- Use the executor framework for executing unrelated tasks.
- Use nonblocking solutions when possible.
- Several library classes are not thread safe.

#### Thread-Safe Collections
ReentrantLock
A lock that can be acquired in one method and released in another.
Semaphore
An implementation of the classic semaphore, a lock with a count.
CountDownLatch
A lock that waits for a number of events before releasing all threads waiting on it. This allows all threads to have a fair chance of starting at about the same time.

#### Know Your Execution Models
Bound Resources
Resources of a fixed size or number used in a concurrent environ- ment. Examples include database connections and fixed-size read/ write buffers.
Mutual Exclusion
Only one thread can access shared data or a shared resource at a time.
Starvation
One thread or a group of threads is prohibited from proceeding for an excessively long time or forever. For example, always let- ting fast-running threads through first could starve out longer run- ning threads if there is no end to the fast-running threads.
Deadlock
Two or more threads waiting for each other to finish. Each thread has a resource that the other thread requires and neither can finish until it gets the other resource.
Livelock
Threads in lockstep, each trying to do work but finding another "in the way." Due to resonance, threads continue trying to make progress but are unable to for an excessively long time— or forever.

#### Producer-Consumer

#### Readers-Writers

#### Dining Philosophers

### Beware Dependencies Between Synchronized Methods
- Client-Based Locking—Have the client lock the server before calling the first method and make sure the lock’s extent includes code calling the last method.
- Server-Based Locking—Within the server create a method that locks the server, calls all the methods, and then unlocks. Have the client call the new method.
- Adapted Server—create an intermediary that performs the locking. This is an exam- ple of server-based locking, where the original server cannot be changed.

### Keep Synchronized Sections Small

### Writing Correct Shut-Down Code Is Hard
Think about shut-down early and get it working early. 
It’s going to take longer than you expect. Review existing algorithms because this is probably harder than you think.

### Testing Threaded Code
Write tests that have the potential to expose problems and then run them frequently, with different programatic configurations and system configurations and load. 
If tests ever fail, track down the failure. Don’t ignore a failure just because the tests pass on a subsequent run.

- Treat spurious failures as candidate threading issues.
- Get your nonthreaded code working first.
- Make your threaded code pluggable.
- Make your threaded code tunable.
- Run with more threads than processors.
- Run on different platforms.
- Instrument your code to try and force failures.


## Chapter 14 - Successive Refinement

It is that programming is a craft more than it is a science.
To write clean code, you must first write dirty code and then clean it.


We learned this truth in grade school when our teachers tried (usually in vain) to get us to write rough drafts of our compositions. 
The process, they told us, was that we should write a rough draft, then a second draft, then several subsequent drafts until we had our final version. Writing clean compositions, they tried to tell us, is a matter of successive refinement.
Once it’s "working," they move on to the next task, leaving the "working" program in whatever state they finally got it to "work." 
Most seasoned programmers know that this is professional suicide.

Bad schedules can be redone, bad requirements can be redefined. 
Bad team dynamics can be repaired. But bad code rots and ferments, becoming an inexorable weight that drags the team down.
So the solution is to continuously keep your code as clean and simple as it can be. Never let the rot get started.


## Chapter 15 - JUnit Internals

## Chapter 16 - Refactoring SerialDate

### First, Make It Work
### Then Make It Right

So once again we’ve followed the Boy Scout Rule. We’ve checked the code in a bit cleaner than when we checked it out. 
It took a little time, but it was worth it.

## Chapter 17 - Smells and Heuristics

### Comments
- Inappropriate Information
- Obsolete Comment
- Redundant Comment
- Poorly Written Comment
- Commented-Out Code

### Environment
- Build Requires More Than One Step
- Test Require More Than One Step

### Functions
- Too Many Arguments
- Output Arguments
- Flag Arguments
- Dead Function

### General
- Multiple Languages in One Source File
- Obvious Behavior Is Unimplemented
- Incorrect Behavior at the Boundaries
- Overridden Safeties
- Duplication
- Code at Wrong Level of Abstraction
- Base Classes Depending on Their Derivatives
- Too Much Information
- Dead Code
- Vertical Separation
- Inconsistency
- Clutter
- Artificial Coupling
- Feature Envy
- Selector Arguments
- Obscured Intent
- Misplaced Responsibility
- Inappropriate Static
- Use Explanatory Variables
- Function Names Should Say What They Do
- Understand the Algorithm
- Make Logical Dependencies Physical
- Prefer Polymorphism to If/Else or Switch/Case
- Follow Standard Conventions
- Replace Magic Numbers with Names Constants
- Be Precise
- Structure over Convention
- Encapsulate Conditionals
- Avoid Negative Conditionals
- Functions Should Do One Thing
- Hidden Temporal Couplings
- Don't Be Arbitrary
- Encapsulate Boundary Conditions
- Functions Should Descend Only One Level of Abstraction
- Keep Configurable Data at High Levels
- Avoid Transitive Navigation

### Java
- Avoid Long Import Lists by Using WildCards
- Don't Inherit Constants
- Constants versus Enums
