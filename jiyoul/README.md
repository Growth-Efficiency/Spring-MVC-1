※ 1일차 ※

웹 서버 (Web Server)
- HTTP 기반으로 동작.
- 정적 리소스, 기타 부가기능
- 정적 파일(HTML, CSS, JS, 이미지, 영상) 제공
- 예) NGINX, APACHE

웹 애플리케이션 서버 (WAS - Web Application Server)
- HTTP 기반으로 동작
- 웹 서버 기능 포함 + 정적 리소스 제공 가능
- 프로그램 코드를 실행 해서 애플리케이션 로직 수행
  -> 동적 HTML, HTTP API(JSON)
  -> 서블릿, JSP, 스프링 MVC
- Tomcat, Jetty, Undertow

서블릿 응답 흐름
1. WAS는 웹브라우저로 부터 HTTP 요청을 받을시 Request, Response 객체를 새로 만들어서 호출함.
2. 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내서 사용할 수 있음.
3. 개발자는 응답 정보도 Response 객체에서 참조하여 만들어준 Response에 편리하게 입력 가능.
4. WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답 정보를 생성함.

웹브라우저로 부터 요청이 올때 마다 쓰레드 생성할 시 장단점

장점
- 동시 요청을 처리 할 수 있다.
- 리소스(CPU, 메모리)가 허용 할 때 까지 처리가능
- 하나의 쓰레드가 지연되어도, 나머지 쓰레드는 정상 동작이 된다.
  단점
- 쓰레드는 생성 비용은 매우 비싸다.
  -> 고객의 요청이 올 때 마다 쓰레드를 생성하면, 응답 속도가 늦어진다.
- 쓰레드는 컨텍스트 스위칭 비용이 발생한다.
  -> CPU에 쓰레드를 번갈아가며 처리할때 (스위칭) 비용이 발생한다.
- 쓰레드 생성에 제한이 없다.
  -> 고객 요청이 너무 많이 오면 CPU, 메모리 임계점을 넘어서 서버가 죽을 수 있다.

쓰레드 풀
-  필요한 쓰레드를 쓰레드 풀에 보관하고 관리함.
   -> 요청마다 쓰레드의 생성의 단점을 보완.
- 쓰레드의 최대치 갯수를 지정해 놓았는데 그이상 호출시 쓰레드 대기, 거절 될 수 있다.(특정 숫자 만큼만 대기 하도록 설정가능.)
- 쓰레드가 필요시 쓰레드 풀에 이미 생성되있는 쓰레드를 가져가고, 다쓰면 쓰레드 풀로 다시 반납함.
- 쓰레드가 미리 생성되어 있으므로, 쓰레드를 생성하고 종료 하는 비용(CPU)이 절약되고 빠름.
- 너무 많은 요청이 와도 최대치가 있기 때문에 안전하게 처리가 가능함.

WAS는 멀티 쓰레드에 대한 부분은 다 지원해주므로 개발자는 멀티 쓰레드 관련 코드에 대해 신경 쓰지 않아도 됌.
-> 단, 멀티 쓰레드 환경이므로 싱글톤 객체(서블릿, 스프링 빈)는 주의 해서 사용해야 함.

HTTP 주고 받을때 백앤드 개발자가 고민해야 할 것.
- 정적리소스 제공
- 동적 HTML 페이지
- HTTP API

서버 사이드 렌더링(SSR)
- 서버에서 최종 HTML을 생성해서 전달.
- 주로 정적인 화면에 사용.
- JSP, 타임리프
- 백엔드 개발자는 서버 사이드 렌더링 기술 학습하기.(타임리프 해보자)

클라이언트 사이드 렌더링
- HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용함.
- 주로 동적인 화면에 사용, 웹 환경을 마치 앱처럼 필요한 부분부분 변경 할 수 있음.
- 구글 지도, Gmail, 구글 캘린더
- React, Vue.js
- 클라이언트 사이드 렌더링 순서
1. (웹 브라우저) 서버로 order.html을 요청함
2. (서버) 웹 브라우저로 빈 HTML과 자바스크립트 링크를 전송함.
3. (웹 브라우저) 서버로 자바스크립트를 요청함.
4. (서버) 웹 브라우저로 JavaScript(클라이언트 로직, HTML 렌더링 코드가 들어있음)을 전송함.
5. (웹 브라우저) 서버로 HTTP API 데이터 요청함.
6. (서버) JSON으로 해당 데이터를 전송해줌.
7. (웹 브라우저) 이전에 받은 JavaScript로 HTML 만들어 결과를 렌더링함.

스프링 웹 기술
Spring Boot
- WAS 서버를 내장하고있음.
  Web Servlet
- 애노테이션 기반의 스프링 MVC 사용.

Web Reactive (최신기술)
- 스프링 웹 플럭스(Spring WebFlux)
- 비동기 넌 블러킹 처리.
- 최소한의 쓰레드로 최대 성능을 냄.(쓰레드의 컨텍스트 스위칭 비용이 효율적임.)
- 함수형 스타일로 개발하여 동시처리 코드 효율화
- 서블릿 기술을 사용하지 않음.
- 기술적 난이도가 매우 높음.
- 아직 관계형 데이터 베이스(RDB) 지원이 부족함.
- 아직은 실무에서 많이 사용하지 않음.

자바 뷰 템플릿 역사
JSP
- 속도가 느림, 기능부족
  프리마커
- 속도 문제 해결, 다양한 기능
  타임리프
- 내추럴 템플릿 : HTML의 모양을 유지하면서 뷰 템플릿 적용 가능.
- 스프링 MVC와 강력한 기능 통합
- 최선의 선택, 단 성능은 프리마커, 벨로시티가 더 빠름

------------------------------------------------------------------------------------------------------------------------------------------

※ 2일차 ※

HTTP 요청 메시지 로그 콘솔로 확인하기.
-> application.properties에 logging.level.org.apache.coyote.http11=debug 입력
-> 성능저하 발생할 수 있으니 개발단계에서만 사용하자.

GET방식 Query 파라미터 값 받아올때.
HttpServletRequest request;
request.getParameter('name'); 중복시 첫번째 값을 반환함.
request.getParameterValues('name') 하면 중복 파라미터 값을 배열로 반환함.(중복으로는 잘 사용안함.)

POST HTML Form형식으로 데이터를 전송할땐 HTTP 메시지 바디에 해당 데이터를 포함해서 보내기때문에
바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정해 주어야 한다. 'application/x-www-form-urlencoded'

------------------------------------------------------------------------------------------------------------------------------------------

※ 3일차 ※

공부해야할것 : HashMap과 ConcurrentHashMap 차이

템플릿 엔진 : JSP, Thymeleaf, Freemarker, Velocity

서블릿으로 개발할때는 뷰화면을 위한 HTML을 만드는 작업이 자바코드에 섞여서 지저분해짐.
JSP로 개발할때는 뷰를 생성하는 HTML 작업을 깔끔하게 가져가고, 중간중간 동적으로 변경이 필요한 부분에만 자바 코드를 적용함.

redirect vs forward 차이
-> 리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시
요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. 반면에 포워드는
서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.

여태 직접 만들었던것과 스프링 MVC 비교
FrontController	  -> DispatcherServlet
handlerMappingMap -> HandlerMapping
MyHandlerAdapter   -> HandlerAdapter
ModelView	  -> ModelAndView
viewResolver	  -> ViewResolver
MyView		  -> View


------------------------------------------------------------------------------------------------------------------------------------------

※ 4,5일차 ※
- 코딩
------------------------------------------------------------------------------------------------------------------------------------------

※ 6일차 ※

Spring MVC 동작 순서

1. HTTP 요청(클라이언트) : HTTP 요청을 보냄.
2. 핸들러 조회(Dispatcher Servlet) : 응답을 받아 핸들러 매핑을 통해 HTTP요청 URL(헤더라던지 다른정보 더 확인함)에 매핑된 핸들러(컨트롤러)를 조회.
* 핸들러매핑은 스프링부트가 미리 등록을 해놓음.
3. 핸들러 어댑터 조회(Dispatcher Servlet) : 조회한 핸들러를 처리할 수 있는 핸들러 어댑터를 조회.
4. 핸들러 어댑터 실행(Dispatcher Servlet) : 조회한 핸들러 어댑터를 실행한다.
5. 핸들러 실행(Handler Adapter) : 핸들러 어댑터가 조회한 실제 핸들러(Controller)를 실행 한다.
6. ModelAndView 반환(Handler Adapter) : 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.
* 핸들러가 뭘 반환하든 어떻게만 ModelAndView로 만들어서 반환해줌.
7. viewResolver 호출(Dispatcher Servlet) : 뷰 리졸버를 찾아 실행핸다.
* JSP의 경우 'InternalResourceViewResolver'가 자동 등록되고 사용된다.
8. View 반환(viewResolver) : 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
* JSP의 경우 'InternalResourceView(JstView)'를 반환하는데, 내부에 'forward()' 로직이 있다.
9. View 렌더링(Dispatcher Servlet -> View) : 뷰를 통해서 뷰를 렌더링 한다.
* 다른 뷰는 실제 뷰를 렌더링 하지만 JSP의 경우 'forward()'를 통해서 해당 JSP로 이동해야 렌더링이 된다.

------------------------------------------------------------------------------------------------------------------------------------------

※ 7일차 ※

@RestController는 반환할때 String으로 반환 받으면 그 이름으로 뷰를 찾지않고 그냥 바로 그 String을 HTTP 메시지 바디에 던져준다.

LOG 레벨 : TRACE > DEBUG > INFO > WARN > ERROR
개발서버는 debug로 운영서버는 info로 보통 출력함.

application.properties에 LOG 보여줄 레벨 설정 하는법.
- 전체 로그 레벨 설정 (내 패키지의 모든 기본설정 = info) : logging.level.root=info
- 원하는 패키지와 그 하위 로그 레벨 설정 (더 깊숙하므로 우선권 가짐) : logging.level.hello.springmvc=debug

@RequestMapping은 url을 다중 설정도 가능하다.
- @RequestMapping({"hello", "bye"})

@PathVariable 하면 URL꺼 빼올수 있음. (다중도 가능)
-> @GetMapping("/mapping/user/{userId}/orders/{orderId}")
-> public String mapping(@PathVariable String userId, @PathVariable String orderId)

------------------------------------------------------------------------------------------------------------------------------------------

※ 8일차 ※
코딩
------------------------------------------------------------------------------------------------------------------------------------------

※ 9일차 ※

요청 파라미터를 조회하는 기능: @RequestParam , @ModelAttribute
HTTP 메시지 바디를 직접 조회하는 기능: @RequestBody
헤더 정보가 필요하다면 : HttpEntity<DTO> 사용해서 꺼내기 또는 @RequestHeader

스프링 부트는 클래스패스의 다음 디렉토리에 있는 정적 리소스를 제공해준다.
/static, /public, /resources, /META-INF/resources
ex) src/main/resources/static/basic/hello-form.html 이면
-> http://localhost:8080/basic//hello-form.html 처럼 실행하면 된다.

타임리프 bulid.gradle에 추가했으면
아래와 같은 값이 디폴트로 application.properties에 추가된다. (변경하고 싶으면 아래값을 넣고 경로 변경하면 됨.)
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html

RequestMappingHandlerAdapter 동작 방식 설명.
1. Dispatcher에서 핸들러를 찾고 -> 핸들러 어댑터를 찾고 -> 리퀘스트 매핑 핸들러어댑터를 실행한다.
2. 이때 핸들러 어댑터는 Argument Resolver(HandlerMethodArgumentResolver)를 호출하여 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터를 생성한다.
3. 다양한 파라미터(@RequestBody, HttpEntity, Model  등)를 생성할때 Argument Resolver가 HTTP 메세지 컨버터를 호출하여 요청 데이터를 생성한다.
4. 생성한 파라미터들을 컨트롤러(핸들러)에 넘기고 다시 응답(response)를 할때 ReturnValueHandler를 호출한다.
5. 응답 데이터(@ResponseBody, HttpEntity, ModelAndView  등)를 또 HTTP메세지 컨버터를 사용하여 다시 만들어 핸들러 어댑터로 반환한다.

Spring 기능 확장 하고 싶을때는 WebMvcController를 확장하면 된다.(그 안에 엄청 많이 확장할 수 있게 되어있음)

------------------------------------------------------------------------------------------------------------------------------------------

※ 10일차 ※

싱글톤을 보장하는 곳에서 여러군데에서 사용할땐 아래와 같이 동시성을 보장하는 것으로 해주어야 한다.
HashMap -> ConcurrentHashMap, Long -> AtomicLong

타임리프
변수 표현식 : ${param}
URL링크 표현식 : @{URL}
반복 표현식 : th:each
내용 변경 : th:text
속성 변경 : th:onclick
데이터 변경 : th:value

th:href="@{/basic/items/${itemId}(itemId=${item.id})}" 를 아래와 같이 리터럴 대체 문법으로 바꿀수 있다.
-> th:href="@{|/basic/items/${item.id}|}"

------------------------------------------------------------------------------------------------------------------------------------------

※ 11일차 ※

스프링에선 redirect:/~ 로 편리하게 리다이렉트를 사용할 수 있다.
-> return "redirect:/basic/items/{itemId}"; 이렇게 하면 itemId 이것도 @PathVariable Long itemId의 값으로 들어간다.