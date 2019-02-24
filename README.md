[문서자료](https://docs.google.com/document/d/1V05o1_ZxhHIuN2-zkwxcwIcKpJ7V_XrLaJTAXl9VA54/edit#heading=h.27bngx7yn4d7)

### 1부 Spring MVC 동작원리

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


- DispatcherServlet 동작 원리 2
    - HandlerMapping
        - RequestMappingHandlerMapping (3.1 이상부터 default)
          
    - HandlerAdapter
        - RequestMappingHandlerAdapter (3.1 이상부터 default)

    - HandlerMapping
        - BeanNameUrlHandlerMapping
           
    - HandlerAdapter
        - SimpleControllerHandlerAdapter

- DispatcherServlet 동작 원리 3
    - ViewResolver
        - InternalResourceViewResolver(default)
        
```
@Configuration
@ComponentScan
public class WebConfig {

   @Bean
   public InternalResourceViewResolver viewResolver() {
       InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
       viewResolver.setPrefix("/WEB-INF/");
       viewResolver.setSuffix(".jsp");
       return viewResolver;
   }

}

```

- 스프링 MVC 구성요소
    - MultipartResolver
        - 파일 업로드 요청 처리에 필요한 인터페이스
        - HttpServletRequest를 MultipartHttpServletRequest로 변환해주어 요청이 담고 있는 File을 꺼낼 수 있는 API 제공.
        
    - LocaleResolver
        - 파일 업로드 요청 처리에 필요한 인터페이스
        - HttpServletRequest를 MultipartHttpServletRequest로 변환해주어 요청이 담고 있는 File을 꺼낼 수 있는 API 제공.
       
    - ThemeResolver
        - 애플리케이션에 설정된 테마를 파악하고  변경할 수 있는 인터페이스
        - 참고: https://memorynotfound.com/spring-mvc-theme-switcher-example/

    - HandlerMapping
        - 요청을 처리할 핸들러를 찾는 인터페이스
    
    - HandlerAdapter
        - HandlerMapping이 찾아낸 핸들러를 처리하는 인터페이스
        - 스프링 MVC 확장력의 핵심
    
    - HandlerExceptionResolver
        - 요청 처리 중에 발생한 에러 처리하는 인터페이스
        
    - RequestToViewNameTranslator
        - 핸들러에서 뷰 이름을 명시적으로 리턴하지 않은 경우, 요청을 기반으로 뷰 이름을 판단하는 인터페이스
        
    - ViewResolver
        - 뷰 이름(String)에 해당하는 뷰를 찾아내는 인터페이스
        
    - FlashMapManager
        - FlashMap 인스턴스를 가져오고 저장하는 인터페이스
        - FlashMap 은 주로 리다이렉션을 사용할 때 요청 매개변수를 사용하지 않고 데이터를 전달하고 정리할 때 사용한다
        - redirect:/events
        
- 스프링 동작 원리 정리
    - DispatcherServlet 초기화
        1. 특정 타입에 해당하는 빈을 찾는다.
        2. 없으면 기본전략을 사용한다. (DispatcherServlet.properties)
        
    - 스프링 부트 사용하지 않는 스프링 MVC
        - 서블릿 컨테이너(ex, 톰캣)에 등록한 웹 애플리케이션(WAR)에 DispatcherServlet을 등록한다
            - web.xml에 서블릿 등록
            - 또는 WebApplicationInitializer에 자바 코드로 서블릿 등록
            
    - 스프링 부트를 사용하는 스프링 MVC
        -자바 어플리케이션에 내장 톰캣을 만들고 그 안에 DispatcherServlet을 등록한다
            - 스프링 부트 자동 설정이 자동으로 해줌
        - 스프링 부트의 주관에 따라 여러 인터페이스 구현체를 빈으로 등록한다
       
## 2부 스프링 MVC 설정
     
### 스프링 MVC 빈 설정

- @EnableWebMvc
    - 애노테이션 기반 스프링 MVC 사용할 때 편리한 웹 MVC 기본 설정
    - requestMappingHandler에서 RequestMappingHandlerMapping을 BeanNameUrlMappingHandler 보다 order를 높게줌 
    - 기본적으로 사용되는 인터셉터도 추가해줌
    - messageConverter 도 기본적으로 추가 해줌 classpath에 따라 필요클래스도 동적으로 추가해줌
    - 기본적으로 사용하는 viewResolver 등록

### WebMvcConfigurer

- @EnableMvc가 제공하는 빈을 커스터마이징 할 수 있는 기능을 제공하는 인터페이스
    
    ```java
    @Configuration
    @EnableWebMvc
    public class WebConfig implements WebMvcConfigurer {
    
       @Override
       public void configureViewResolvers(ViewResolverRegistry registry) {
           registry.jsp("/WEB-INF/", ".jsp");
       }
    }
    ```

###스프링 부트의 스프링 MVC 설정 

- 스프링 MVC 커스터마이징
    - application.properties
        - ResourceProperties.java 에 prefix 로 몇가지 설정 되어있음
    - @Configuration + implements WebMvcConfigurer
        - 스프링 부트의 스프링 MVC 자동 설정 + 추가 설정
        - 스프링 부트면 필요한것들을 빈으로 등록하면 알아서 등록해줌 아니면 WebMvcConfigurer 에서 오버라이딩해서 재정의 해도 된다
    - @Configurationn + @EnableWebMvc + implements WebMvcConfigurer
        - 스프링 부트의 스프링 MVC 자동설정 사용하지 않음
        
### 스프링 부트에서 JSP 사용하기

- JAR 프로젝트로 만들 수 없은. WAR 프로젝트로 만들어아햠
- JAVA -JAR 로 실행 할 수는 있지만 실행가능한 JAR파일은 지원하지 않음 
- webapp 밑에 WEB-INF밑에 jsp 디렉토리 구조
- application.properties
    ```properties
      spring.mvc.view.prefix = /WEB-INF/jsp/
      spring.mvc.view.suffix = .jsp
    ```
    
- 의존성 추가
    ```xml
      <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
      </dependency>
      <dependency>
         <groupId>org.apache.tomcat.embed</groupId>
         <artifactId>tomcat-embed-jasper</artifactId>
         <scope>provided</scope>
      </dependency>
    ``` 

### WAR 파일 배포하기

- war:exploded 
    - war를 풀어서 배포
- war
    - war로 묶어서 배포
    
    
### WebMvcConfigurer 설정 : formatter 설정

- Formatter
    - Printer
        - 해당 객체를 (Locale 정보를 참고하여) 문자열로 어떠게 출력할 것인가
    - Parser
        - 어떤 문자열을 (Locale 정보를 참고하여) 객체로 어떻게 변환할 것인가
        
- 추가하는 방법
    - WebMvcConfigurer 의 addFormatters 메소드 정의하거나
    - 해당 포매터를 빈으로 등록 (스프링 부트에서만 가능)
    
### 도메인 클래스 컨버터

- spring data jpa의 도메인 클래스 컨버터를 활용하면 
- Formatter을 따로 등록 안하고 @requestParma("id") Person person 맵핑 할 수 있다
    - 스프링 데이터 JPA가 제공하는 Repository를 사용하여 ID에 해당하는 엔티티를 읽어옵니다
    
        

    
 
        
        
    

    
 
 
 