### Spring MVC

- 서블릿(Servlet)
    - 웹 애플리케이션 개발용 스팩과 API 제공
    - 요청 당 쓰레드 만들거나 풀에서 가져와서 사용
    - HttpServlet이 중요한 클래스
    
- 서블릿 장점
    - 빠르다
    - 플랫폼 독립적
    - 보안
    - 이식성
    
- 서블릿 엔진 또는 서블릿 컨테이너(tomcat, jetty)
    - 세션 관리
    - 네트워크 서비스
    - MIME 기반 메시지 인코딩 디코딩
    - 서블릿 생명주기 관리
    
- 서블릿 생명주기
    - 서블릿 컨테이너가 서블릿 인스턴스 초기화할때
        - init()
    - 클라이언트 요청 처리 
        - service() -> doGet() || doPost()
    - 서블릿 메모리에서 내릴때
        - destroy()
    
    

- 서블릿 리스너
    - 주요 이벤트를 감지
        - 서블릿 컨텍스트 수준의 이벤트
            - 컨텍스트 라이프사이클 이벤트
            - 컨텍스트 애트리뷰트 변경 이벤트
        - 세션 수준의 이벤트
            - 세션 라이프사이클 이벤트
            - 세션 애트리뷰트 변경 이벤트

- 서블릿 필터
    - 서블릿 컨테이너와 서블릿 사이의 처리에서 사용
    
    
- 서블릿 애플리케이션에 스프링 연동하기
    - 서블릿에서 스프링이 제공하는 IoC 컨테이너 활용하는 방법
        - ContextLoaderListener
            - 애플리케이션 컨텍스트를 만들어서 서블릿 컨텍스트에 등록
    - 스프링이 제공하는 서블릿 구현체 DispatcherServlet 사용하기
        
- [웹 어플리케이션 동작원리](https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/)



- DispatcherServlet 동작 원리
    - 다음의 특별한 타입의 빈들을 찾거나, 기본 전략에 해당하는 빈들을 등록한다.
        - HandlerMapping: 핸들러를 찾아주는 인터페이스
        - HandlerAdapter: 핸들러를 실행하는 인터페이스
        
        
- DispatcherServlet 동작 순서
    1. 요청을 분석한다. (로케일, 테마, 멀티파트 등)
    2. (핸들러 맵핑에게 위임하여) 요청을 처리할 핸들러를 찾는다. 
    3. (등록되어 있는 핸들러 어댑터 중에) 해당 핸들러를 실행할 수 있는 “핸들러 어댑터”를 찾는다.
    4. 찾아낸 “핸들러 어댑터”를 사용해서 핸들러의 응답을 처리한다.
        핸들러의 리턴값을 보고 어떻게 처리할지 판단한다.
            뷰 이름에 해당하는 뷰를 찾아서 모델 데이터를 랜더링한다.
            @ResponseEntity가 있다면 Converter를 사용해서 응답 본문을 만들고.
    5. (부가적으로) 예외가 발생했다면, 예외 처리 핸들러에 요청 처리를 위임한다.
    6. 최종적으로 응답을 보낸다.



        