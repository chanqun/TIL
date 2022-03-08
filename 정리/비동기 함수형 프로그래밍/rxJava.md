[inflearn rxJava](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EB%A6%AC%EC%95%A1%ED%8B%B0%EB%B8%8C%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-1/lecture/41592


## reactive programming?

> In computing, reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.

Push 방식 : 데이터의 변화가 발생했을 때 변경이 발생한 곳에서 데이터를 보내주는 방식

- RTC
- 소켓 프로그래밍
- DB Trigger
- Spring의 ApplicationEvent
- Angular의 데이터 바인딩
- 스마트폰의 push 메시지

Push 방식 : 변경된 데이가 있는지 요청을 보내 질의하고 변경된 데이터를 가져오는 방식

- 클라이언트 요청 & 서버 응답 방식의 애플리케이션



> Observable : 데이터 소스
> 리액티브 연산자(Operators) : 데이터 소스를 처리하는 함수
> 스케줄러(Scheduler) : 스레드 관리자
> 함수형 프로그래밍 : RxJava에서 제공하는 연사자 함수를 사용