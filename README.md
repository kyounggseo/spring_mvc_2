:book: 스프링 MVC - 기본 기능 <br/>

✏️ 로깅 간단히 알아보기 <br/> 
:round_pushpin: SLF4J <br/> 
운영 시스템에서는 System.out.println() 같은 시스템 콘솔을 사용해서 필요한 정보를 출력하지 않고, 별도의 로깅 라이브러리를 사용해서 로그를 출력한다. <br/> 
스프링 부트 로깅 라이브러리는 기본으로 다음 로깅 라이브러리를 사용한다. <br/>
:one: SLF4J - http://www.slf4j.org<br/>
:two: Logback - http://logback.qos.ch<br/>
로그 라이브러리는 Logback, Log4J, Log4J2 등등 수 많은 라이브러리가 있는데, 그것을 통합해서 인터페이스로 제공하는 것이 바로 SLF4J 라이브러리다.<br/>
쉽게 이야기해서 SLF4J는 인터페이스이고, 그 구현체로 Logback 같은 로그 라이브러리를 선택하면 된다.<br/>
실무에서는 스프링 부트가 기본으로 제공하는 Logback을 대부분 사용한다.<br/>
:round_pushpin: 로그 선언<br/>
:one: private Logger log = LoggerFactory.getLogger(getClass());<br/>
:two: private static final Logger log = LoggerFactory.getLogger(Xxx.class)<br/>
:three: @Slf4j : 롬복 사용 가능<br/>
:round_pushpin: 로그 호출<br/>
>> log.info("hello")<br/>
<br/>
✏️ 매핑 정보<br/>
:round_pushpin: @RestController<br/>
>> @Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.<br/>
>> @RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다. <br/>
따라서 실행 결과로 메세지를 받을 수 있다.<br/>
:round_pushpin: LEVEL: TRACE > DEBUG > INFO > WARN > ERROR<br/>
>> 개발 서버는 debug 출력<br/>
>> 운영 서버는 info 출력<br/>
<br/>
✏️ 요청 매핑<br/>
:round_pushpin: @RequestMapping("/hello-basic")<br/>
>> /hello-basic URL 호출이 오면 이 메서드가 실행되도록 매핑한다.<br/>
>> 대부분의 속성을 배열[] 로 제공하므로 다중 설정이 가능하다. {"/hello-basic", "/hello-go"}<br/>
<br/>

✏️ HTTP 요청 파라미터 - 쿼리 파라미터, HTML Form<br/>
:round_pushpin: HTTP 요청 데이터 조회 <br/>
HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법을 알아보자.<br/>
클라이언트에서 서버로 요청 데이터를 전달할 때는 주로 다음 3가지 방법을 사용한다.<br/>
:one: GET - 쿼리 파라미터<br/>
/url?username=hello&age=20<br/>
메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달<br/>
예) 검색, 필터, 페이징등에서 많이 사용하는 방식<br/>
:two: POST - HTML Form<br/>
content-type: application/x-www-form-urlencoded<br/>
메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20<br/>
예) 회원 가입, 상품 주문, HTML Form 사용<br/>
:three: HTTP message body에 데이터를 직접 담아서 요청<br/>
HTTP API에서 주로 사용, JSON, XML, TEXT<br/>
데이터 형식은 주로 JSON 사용<br/>
POST, PUT, PATCH<br/>
<br/>
✏️ 요청 파라미터 - 쿼리 파라미터, HTML Form<br/>
:round_pushpin: HttpServletRequest 의 request.getParameter() 를 사용하면 다음 두가지 요청 파라미터를 조회할 수 있다<br/>
GET 쿼리 파리미터 전송 방식이든, POST HTML Form 전송 방식이든 둘다 형식이 같으므로 구분없이 조회할 수 있다.<br/>
이것을 간단히 >> 요청 파라미터(request parameter) 조회 << 라 한다.<br/>
:round_pushpin: 스프링이 제공하는 @RequestParam 을 사용하면 요청 파라미터를 매우 편리하게 사용할 수 있다.<br/>
<br/>
:one: @RequestParam : 파라미터 이름으로 바인딩<br/>
:two: @ResponseBody : View 조회를 무시하고, HTTP message body에 직접 해당 내용 입력<br/>
:three: @RequestParam의 name(value) 속성이 파라미터 이름으로 사용<br/>
:four: @RequestParam("username") String memberName == request.getParameter("username")<br/>
:five: String , int , Integer 등의 단순 타입이면 @RequestParam 도 생략 가능<br/>
<br/>
✏️ HTTP 요청 파라미터 - @ModelAttribute<br/>
요청 파라미터를 받아서 필요한 객체를 만들고 그 객체에 값을 넣어주어야 한다. 스프링은 이 과정을 완전히 자동화해주는 @ModelAttribute 기능을 제공한다.<br/>
<br/>
:one: 롬복 @Data<br/>
  >> @Getter , @Setter , @ToString , @EqualsAndHashCode , @RequiredArgsConstructor 를 자동으로 적용해준다.<br/>
<br/>
:two: 스프링MVC는 @ModelAttribute 가 있으면 다음을 실행한다.<br/>
:one: HelloData 객체를 생성한다.<br/>
:two: 요청 파라미터의 이름으로 HelloData 객체의 프로퍼티를 찾는다. 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩) 한다. 예) 파라미터 이름이 username 이면 setUsername() 메서드를 찾아서 호출하면서 값을 입력한다.<br/>
<br/>
:three: @ModelAttribute 는 생략할 수 있다.<br/>
그런데 @RequestParam 도 생략할 수 있으니 혼란이 발생할 수 있다. 스프링은 해당 생략시 다음과 같은 규칙을 적용한다.<br/>
  :one: String , int , Integer 같은 단순 타입 = @RequestParam<br/>
  :two: 나머지 = @ModelAttribute (argument resolver 로 지정해둔 타입 외<br/>
✏️ 프로퍼티란?<br/>
객체에 getUsername() , setUsername() 메서드가 있으면, 이 객체는 username 이라는 프로퍼티를 가지고 있다.<br/>
username 프로퍼티의 값을 변경하면 setUsername() 이 호출되고, 조회하면 getUsername() 이 호출된다.<br/>
<br/>
✏️ HTTP 요청 메시지 - 단순 텍스트<br/>
요청 파라미터와 다르게, HTTP 메시지 바디를 통해 데이터가 직접 넘어오는 경우는 @RequestParam , @ModelAttribute 를 사용할 수 없다. <br/>
(물론 HTML Form 형식으로 전달되는 경우는 요청 파라미터로 인정된다.)<br/>
>> HTTP 메시지 바디의 데이터를 InputStream 을 사용해서 직접 읽을 수 있다<br/>
<br/>

:one: @RequestBody<br/>
@RequestBody 를 사용하면 HTTP 메시지 바디 정보를 편리하게 조회할 수 있다. 참고로 헤더 정보가 필요하다면 HttpEntity 를 사용하거나 @RequestHeader 를 사용하면 된다.<br/>
이렇게 메시지 바디를 직접 조회하는 기능은 요청 파라미터를 조회하는 @RequestParam , @ModelAttribute 와는 전혀 관계가 없다.<br/>
:two: 요청 파라미터 vs HTTP 메시지 바디<br/>
요청 파라미터를 조회하는 기능: @RequestParam , @ModelAttribute<br/>
HTTP 메시지 바디를 직접 조회하는 기능: @RequestBody<br/>
:three: @ResponseBody<br/>
@ResponseBody 를 사용하면 응답 결과를 HTTP 메시지 바디에 직접 담아서 전달할 수 있다. 물론 이 경우에도 view를 사용하지 않는다<br/>
:four: @RequestBody 객체 파라미터<br/>
@RequestBody HelloData data<br/>
@RequestBody 에 직접 만든 객체를 지정할 수 있다.<br/>
HttpEntity , @RequestBody 를 사용하면 HTTP 메시지 컨버터가 HTTP 메시지 바디의 내용을 우리가 원하는 문자나 객체 등으로 변환해준다.<br/>
@RequestBody는 생략 불가능<br/>
:five: 정리<br/>
@RequestBody 요청<br/>
JSON 요청 HTTP 메시지 컨버터 객체<br/>
@ResponseBody 응답<br/>
객체 HTTP 메시지 컨버터 JSON 응답<br/>
<br/>
✏️ HTTP 응답 - 정적 리소스, 뷰 템플릿<br/>
스프링(서버)에서 응답 데이터를 만드는 방법은 크게 3가지이다.<br/>
:round_pushpin: 정적 리소스<br/>
예) 웹 브라우저에 정적인 HTML, css, js를 제공할 때는, 정적 리소스를 사용한다.<br/>
:round_pushpin: 뷰 템플릿 사용<br/>
예) 웹 브라우저에 동적인 HTML을 제공할 때는 뷰 템플릿을 사용한다.<br/>
:round_pushpin: HTTP 메시지 사용<br/>
HTTP API를 제공하는 경우에는 HTML이 아니라 데이터를 전달해야 하므로, HTTP 메시지 바디에 JSON 같은 형식으로 데이터를 실어 보낸다<br/>
<br/>
✏️ HTTP 메시지 컨버터<br/>
뷰 템플릿으로 HTML을 생성해서 응답하는 것이 아니라, HTTP API처럼 JSON 데이터를 HTTP 메시지 바디에서 직접 읽거나 쓰는 경우 HTTP 메시지 컨버터를 사용하면 편리하다. 스프링 MVC는 다음의 경우에 HTTP 메시지 컨버터를 적용한다.<br/>
:round_pushpin: HTTP 요청: @RequestBody , HttpEntity(RequestEntity) , <br/>
:round_pushpin: HTTP 응답: @ResponseBody , HttpEntity(ResponseEntity)<br/>
<br/>
✏️ 요청 매핑 헨들러 어뎁터 구조<br/>
그렇다면 HTTP 메시지 컨버터는 스프링 MVC 어디쯤에서 사용되는 것일까?<br/>
모든 비밀은 애노테이션 기반의 컨트롤러, 그러니까 @RequestMapping 을 처리하는 핸들러 어댑터인 RequestMappingHandlerAdapter (요청 매핑 헨들러 어뎁터)에 있다.<br/>
![image](https://user-images.githubusercontent.com/102573192/217997014-3142bf3f-11b8-421f-9466-884971b76a84.png)<br/>
<br/>
:one: ArgumentResolver<br/>
생각해보면, 애노테이션 기반의 컨트롤러는 매우 다양한 파라미터를 사용할 수 있었다.<br/>
HttpServletRequest , Model 은 물론이고, @RequestParam , @ModelAttribute 같은 애노테이션 그리고 @RequestBody , HttpEntity 같은 HTTP 메시지를 처리하는 부분까지 매우 큰 유연함을 보여주었다. 이렇게 파라미터를 유연하게 처리할 수 있는 이유가 바로 ArgumentResolver 덕분이다.<br/>
애노테이션 기반 컨트롤러를 처리하는 RequestMappingHandlerAdapter 는 바로 이 ArgumentResolver 를 호출해서 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터의 값(객체)을 생성한다. 그리고 이렇게 파리미터의 값이 모두 준비되면 컨트롤러를 호출하면서 값을 넘겨준다.<br/>
:two: 동작 방식<br/>
ArgumentResolver 의 supportsParameter() 를 호출해서 해당 파라미터를 지원하는지 체크하고, 지원하면 resolveArgument() 를 호출해서 실제 객체를 생성한다. 그리고 이렇게 생성된 객체가 컨트롤러 호출시 넘어가는 것이다<br/>
:three: ReturnValueHandler<br/>
HandlerMethodReturnValueHandler 를 줄여서 ReturnValueHandler 라 부른다. ArgumentResolver 와 비슷한데, 이것은 응답 값을 변환하고 처리한다. 컨트롤러에서 String으로 뷰 이름을 반환해도, 동작하는 이유가 바로 ReturnValueHandler 덕분이다<br/>
<br/>
✏️ HTTP 메시지 컨버터<br/>
![image](https://user-images.githubusercontent.com/102573192/217997289-af8273dc-529b-4f4a-abf0-266fca42c52a.png)<br/>
>> HTTP 메시지 컨버터를 사용하는 @RequestBody 도 컨트롤러가 필요로 하는 파라미터의 값에 사용된다.<br/>
>> @ResponseBody 의 경우도 컨트롤러의 반환 값을 이용한다.<br/>
:round_pushpin: 요청의 경우 @RequestBody 를 처리하는 ArgumentResolver 가 있고, HttpEntity 를 처리하는 ArgumentResolver 가 있다. 이 ArgumentResolver 들이 HTTP 메시지 컨버터를 사용해서 필요한 객체를 생성하는 것이다. <br/>
:round_pushpin: 응답의 경우 @ResponseBody 와 HttpEntity 를 처리하는 ReturnValueHandler 가 있다. 그리고 여기에서 HTTP 메시지 컨버터를 호출해서 응답 결과를 만든다.<br/>
:round_pushpin: 스프링 MVC는 @RequestBody @ResponseBody 가 있으면 RequestResponseBodyMethodProcessor (ArgumentResolver),<br/>
:round_pushpin: HttpEntity 가 있으면 HttpEntityMethodProcessor (ArgumentResolver)를 사용한다.<br/>
