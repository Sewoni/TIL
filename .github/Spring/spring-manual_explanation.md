

> "우리가 웹사이트에서 어떤 버튼을 누르거나 주소창에 URL을 입력하면, 그 요청이 **어떻게 서버에 전달되고**, **어떤 코드가 그 요청을 처리하고**, **어떻게 사용자에게 다시 결과가 보여지는지를 보여주는 전체 흐름**이다."

즉, Spring이라는 도구를 활용해 **웹사이트의 뼈대를 직접 짜본 코드**.

---

##  웹사이트가 동작하는 기본 구조
```
1. 사용자: "/greet?name=홍길동" 주소로 요청을 보냄
2. 서버: 어떤 코드를 실행해야 할지 결정함
3. 코드: “홍길동님 안녕하세요!” 라고 응답을 돌려줌
4. 브라우저: 결과를 보여줌
```

이걸 자동으로 처리해주는 도구가 **Spring MVC**.

---

##  전체 흐름도 

```
[브라우저 요청: /greet?name=홍길동]
          ↓
[DispatcherServlet (중앙 문지기)]
          ↓
[HelloController (요청 담당자)]
          ↓
[HelloService (실제 일 처리)]
          ↓
[응답: Hello My name is 홍길동]
```

---

##  코드 하나씩 설명

### 1️⃣ AppConfig.java – Spring 설정 파일
```java
@Configuration // 이 클래스는 "설정 전용 클래스"라는 뜻
@ComponentScan(basePackages = "org.example.manualspring")
// 위 설정은 "해당 폴더 안에서 @Controller, @Service 등을 찾아 자동 등록해줘" 라는 의미
public class AppConfig {
    // 실제 코드는 없지만, Spring한테 “이 프로젝트의 출발점은 여기야” 라고 알려주는 역할
}
```

 사용된 기술:
- @Configuration: 설정 전용 클래스
- @ComponentScan: 스프링이 자동으로 필요한 클래스를 찾아 등록함

---

### 2️⃣ MyWebAppInitializer.java – 프로젝트 시작 설정
```java
public class MyWebAppInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        // 1. Spring에게 설정 파일(AppConfig)을 등록해줘
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class);

        // 2. DispatcherServlet(중앙문지기)을 만들어서, 모든 요청이 여기로 오게 해줘
        DispatcherServlet dispatcherServlet = new DispatcherServlet(context);
        ServletRegistration.Dynamic registration = servletContext.addServlet("dispatcherServlet", dispatcherServlet);
        registration.setLoadOnStartup(1); // 서버 켜지자마자 실행
        registration.addMapping("/"); // 모든 요청 처리하겠다는 뜻

        // 3. EncodingFilter 등록 (한글 깨짐 방지)
        FilterRegistration.Dynamic encodingFilter = servletContext.addFilter("encodingFilter", new EncodingFilter());
        encodingFilter.addMappingForUrlPatterns(null, true, "/*"); // 모든 요청에 필터 적용
    }
}
```

 사용된 기술:
- WebApplicationInitializer: web.xml 없이 Java 코드로 설정 등록
- DispatcherServlet: 모든 웹 요청을 받아주는 Spring의 핵심
- Filter: 요청/응답을 가로채서 인코딩 처리

---

### 3️⃣ EncodingFilter.java – 문자 인코딩 필터
```java
public class EncodingFilter implements Filter {
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) {
        // 문자 깨짐 방지를 위해 UTF-8로 설정함
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        // 다음 필터나 실제 처리 코드로 넘기기
        chain.doFilter(req, resp);
    }
}
```

 사용된 기술:
- Filter: 요청/응답을 사전처리할 수 있게 도와주는 장치

---

### 4️⃣ IHelloService.java – 서비스 인터페이스
```java
public interface IHelloService {
    // 어떤 이름을 넣으면, 인사 메시지를 돌려주는 기능
    String greet(String name);
}
```

📌 사용된 기술:
- 인터페이스: 코드 설계할 때 유연하게 만들기 위한 기술 (교체나 테스트가 쉬워짐)

---

### 5️⃣ HelloService.java – 실제 기능 처리하는 클래스
```java
@Service // 이 클래스는 “서비스”라고 Spring에게 알려주는 역할
public class HelloService implements IHelloService {
    public String greet(String name) {
        // 받은 이름을 그대로 돌려주는 간단한 기능
        return name;
    }
}
```

 사용된 기술:
- @Service: 비즈니스 로직 처리 계층
- 인터페이스 구현: 실무에서 많이 쓰이는 패턴 (유지보수 편함)

---

### 6️⃣ HelloController.java – 요청을 받아 처리하는 코드
```java
@Controller // 이 클래스는 "웹 요청을 처리하는 역할"이라고 Spring에게 알림
@RequestMapping("/") // 기본 주소 설정
public class HelloController {

    private final IHelloService helloService;

    // 생성자에서 서비스 객체를 받는다 → DI(의존성 주입)
    public HelloController(HelloService helloService) {
        this.helloService = helloService;
    }

    // 기본 페이지 요청 처리 ("/")
    @RequestMapping("/")
    @ResponseBody // View 없이 텍스트 바로 반환
    public String index() {
        return "Hello World";
    }

    // 주소: /greet?name=홍길동
    @RequestMapping("/greet")
    @ResponseBody
    public String getHello(@RequestParam(name = "name") String name,
                           HttpServletRequest request,
                           HttpServletResponse response) {
        // 필터 덕분에 여기서 인코딩 처리 안 해도 됨
        return helloService.greet("Hello My name is %s".formatted(name));
    }
}
```

 사용된 기술:
- @Controller: 웹 요청 처리 클래스
- @RequestParam: 사용자가 보낸 파라미터(name) 받기
- @ResponseBody: View 없이 바로 텍스트 응답

---

##  이 코드들이 보여주는 핵심 메시지

| 포인트 | 설명 |
|--------|------|
| Spring의 본질 | DispatcherServlet 중심 구조 이해 |
| 구조적 설계 | Controller → Service → Interface 구조 연습 |
| 설정 자동화가 없는 상황 | Spring Boot 없이 직접 구성 경험 |
| 실제 웹 요청 흐름 이해 | 필터 → Dispatcher → 컨트롤러 → 서비스 → 응답 |

---

##  실무 백엔드 개발자에게 중요한 기술 요소 요약

| 기술 요소 | 이유 |
|------------|---------------------------|
| 의존성 주입 (DI) | 테스트 용이성, 유지보수성 향상 |
| 레이어 분리 | Controller / Service / Repository 분리 설계 훈련 |
| DispatcherServlet 흐름 | Spring MVC 구조 이해에 핵심 |
| Filter 처리 | 사전처리 기술, 실무에서 자주 쓰임 |
| 인터페이스 기반 설계 | SOLID 원칙 적용 및 유연한 구조 |

---
