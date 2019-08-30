JAVA8말고 JAVA9를 써볼까[김형진 님 발표]
=

### JAVA9
1) http/2 client 추가
    - 블로킹, 넌블로킹 모드 지원
    - 자바10에서 java.net 패키지로 이동
    - 자바11에서 java.net.http 패키지로 이동
2) 리액티브 스트림 추가
    - 리액티브 프로그래밍?
        - 이터레이터 + 옵저버 패턴에 아이디를 조합
        - 기본 옵저버블의 문제
            - 마지막 요소인지 아닌지 알 수 없다.
            - 어디서 예외가 났는지 알 수 없다.
            - 리액티브 스트림은 위의 문제를 해결
3) 불변 콜렉션 자료구조
    - 정적 팩토리메소드 제공
    - ex) List.of(...), Set.of(...), Map.of(...)
    - null이 있으면 NullPointerException 발생
    - map의 경우 중복 키가 있으면 IllegalArguementException 발생
4) 직소 프로젝트
    - 모듈 시스템
    - 자신만의 모듈을 만들 수 있다.
    - jmods 에 존재
    - 모듈 시스템이 필요한 이유
        - 일부분만 필요해도 불필요하게 전체 jdk 실행
        - 작은 컴퓨터 디바이스에서 경량화
        - 캡슐화시에 결함 제거(결함이 뭐지?)
        - 모듈 시스템에는 리플렉션을 사용할 수 없음.(컴파일 타임, 런타임시 검사?)
        - 경량화된 이미지 생성 개념
5) private interface method
    - private method
        - 하위 클래스가 재정의 할 수 없으므로 구현부를 지녀야 함
    - private static method
        - private method와 마찬가지.
    - private method는 인터페이스 내에서만 사용 가능하지만 private static method는 다른 static이나 non-static인터페이스에서 사용될 수 있음.
6) try-with-resources
    - 외부 변수를 위드문에 바로 사용 가능
7) 익명 클래스에서 다이아몬드 연산자 사용가능
8) stream() 함수
    - takeWhile
        - false를 리턴전까지의 요소를 취득
    - dropWhile
        - true를 리턴전까지 요소를 모두 버리고 그 이후의 요소를 취득
    - iterate
    - nullable stream
    - Optional class stream method 추가
        - of
        - ifPresentOrElse

### JAVA10
1. var 타입 추가
    - 컴파일 유추 불가
    - 널 초기화, 람다, 배열 초기화 등
2. 다이아몬드 연산자 사용시 Object반환
    - var empty = new ArrayList<>();
    - empty instanceof Object == true


if(Spring AOP == AspectJ) -> {Spring AOP에 대해}[문겸 님 발표]
=

1. Spring AOP != @AspectJ
    - @AspectJ는 Spring AOP의 한 부분
2. Spring AOP는 프록시 기반
    - 프록시 객체는 사용자의 요청(접근)을 제어하기 위한 목적으로 사용하는 객체
    - 프록시 객체는 타겟 객체의 호출시점을 제어 가능
3. JDK에서는 JdkDynamicProxy를 지원
4. Proxy.newProxyInstance(loader, interfaces, handler=interface의 구현 객체)
    - 프록시 객체 생성이 쉬워짐
5. 프록시 패턴과의 차이점
    - 프록시 객체는 하나의 객체 안에 핸들러가 존재
    - 프록시 패턴은 매번 위임 메서드를 생성해줘야 함
6. jdk프록시의 단점 
    - 둘 이상의 핸들러를 등록할 수 없음
    - 여러 타깃에 대한 위임코드 관리가 어렵다
7. Spring AOP는 JdkDynamicProxy의 단점을 해결
    - ProxyFactoryBean
        - 프록시 객체 생성과 동시에 빈으로 등록시켜줌
        - addAdvice() : 여러 핸들러를 추가
            - Advisor = pointcut(advice를 적용할 위치) + advice(순수한 제어 코드)
            - BeforeAdvice
            - AfterAdvice
            - Interceptor
                - MethodInterceptor
                    - 타깃보다 먼저 실행
                    - 타깃에 대한 정보를 받지 않음. 즉 위임코드를 관리할 필요가 없음
8. 동작순서
    1. 프록시팩토리빈에서 프록시 빈 생성
    2. 포인트컷 지정
    3. 어드바이스가 어드바이서 객체에 주입
    4. 어드바이스 실행
    5. 타겟 객체의 메서드 실행
9. AutoProxy
    - 자동으로 타깃 객체를 판별하여 프록시 빈으로 생성
    - BeanPostProcessor(BPP)
10. CGLIB(Code Generator Library)
    - 인터페이스를 클래스타입으로 주입받을 때 문제가 발생. 왜냐면 프록시 객체는 인터페이스를 구현하기 때문
    - 인터페이스를 구현하지 않은 클래스에 대해 프록시를 생성
    - 타깃 클래스 자체를 상속하여 프록시 생성
    - final class나 메서드는 생성 불가
    - 바이트 코드를 조작하여 코드를 생성. 이후 조작한 코드를 계속 사용하여 검증에 대한 부하가 적음
    - JdkDynamicProxy는 검증되지 않은 객체가 들어올 확률이 높음. 이에 매번 호출시 검증을 해야 함.
11. CGLIB이 권장되지 않는 이유
    - 의존성 주입
    - 디폴트 생성자
    - 두번 생성
12. self invocation문제
    - @Transactional이 붙지 않은 메서드에서 @Transactionl이 붙은 메서드를 콜한다면?
    - 트랜잭션이 동작하지 않음
    - 호출의 주체가 어드바이서가 아니기 때문에 어드바이스가 동작하지 않는다.
13. self invocation문제 해결방법
    1) 자기자신을 주입받아 호출
    2) AspectJ 사용
        - AspectJ Compiler가 필요
        - maven에서는 Mojo라는 플러그인이 필요
14. 성능표(빠름 -> 느림)
    1) aspectJ
    2) cglib
    3) spring aop
        - 속도가 느린데도 사용하는 이유 : 컨테이너의 빈을 활용하기 위해


5G 초연결시대에 웹 http의 대안은 QUIC[조홍신 님 발표]
=

1. QUIC(Quicj UDP Internet Connections)
    - 구글이 설계하고 2013년 공개
    - 국제인터넷표준화기구(IETF)에 표준화 제안
    - Google-Quic은 http프로토콜만 지원
    - TLS1.3 암호화 보안 적용
    - 2018년 11월 HTTP/3로 개명
    - 2019년 7월 최종 명세 발표 예정
2. HTTP2
    - headers 프레임, data 프레임으로 나뉨
    - binary형식 인코딩 처리
    - 요청 하나당 하나의 응답으로 처리한 기존 프로토콜과 달리 여러 요청이 있을시 연결된 스트림 내에서 처리
3. QUIC - TLS1.3
    - 커넥션 아이디 존재
        - 병렬스트림 데이터 동시 전송 지원
            - TCP처럼 순서에 맞게 전송 가능
    - 전송중에 누락된 패킷 재전송 기능 지원
    - UDP기반
    - QUIC계층을 추가하여 신뢰성 추가
    - RTT(Round Trip Time)가 0번 또는 1번 발생
        - 핸드쉐이킹이 한 번 일어나면 이후 핸드쉐이킹을 하지 않기 때문

MQ와 amqp프로토콜 원리에 대하여[최경운 님 발표]
=
1. MQ가 필요한 이유
    - 애플리케이션 - 디비 구성의 경우 데이터베이스 쓰기 성능에 영향을 받음
    - 애플리케이션에서 디비 의존성을 제거 해준다.
    - MQ는 시스템 간에 메세지를 전달해주는 서비스를 뜻함
        - 확장이 용이
2. RabbitMQ
    - amqp기반의 오픈소스 메세지 브로커
    - 메세지 브로커는 두 지점을 연결하는 중개자를 말함
3. AMQP(Advanced Message Queuing Protocol)
    - 세 가지 추상 컴포넌트가 존재
        1. exchange
            - 발행한 모든 메세지가 처음 도달하는 지점
            - 메세지 브로커에서 큐에 메세지를 전달
        2. binding
            - 익스체인지에 전달된 메세지가 어떤 큐에 저장되어야 하는지를 정의
            - 바인딩을 사용하여 큐와 익스체인지간의 관계를 정의
        3. queue
            - 메세지를 디스크 또는 메모리에 저장하는 자료구조
4. exchange
    1. direct exchange
        - 라우팅 키를 기반으로 라우팅 키가 정확하게 일치할 경우 메세지 전달
    2. fanout exchange
        - 전체 큐를 대상으로 처리
    3. topic exchange
        - 하나 이상의 큐에 라우팅 키 패턴 기반으로 바인딩
    4. header exchange
        - 메세지 속성중 헤더 속성(x-match)을 사용하여 메세지 처리

5. queue
    - 실제 메세지를 사용하는 게 아니라 참조만 저장. 그러므로 메모리를 적게 사용
    - 메세지가 만료되거나 삭제될 때 다른 큐의 메세지 처리에 영향 없음
    - 설정
        - 자동 삭제
        - 큐 독점
        - 자동 메세지
        - 대기 메세지 수 제한
    - mirrored queue
        - 클러스터 구성시 필요

Spring WebFlux는 어떻게 적은 리소스로 많은 트래픽을 감당할까[김민수 님 발표]
=
카카오 컨퍼런스에서 들었던 토비님의 발표와 많은 부분이 겹쳐 특별히 요약하지 않음.
