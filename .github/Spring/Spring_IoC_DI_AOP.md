
##  1. IoC (제어의 역전) – Inversion of Control

###  쉽게 말하면?
> 원래는 개발자가 모든 것을 직접 통제했다면,  
> 이제는 **Spring이 대신 객체를 만들어주고 관리해주는 것**.

###  비유로 설명!
- **기존 방식**: 내가 필요한 도구를 직접 사러 가야 해!.  
- **IoC 방식**: 필요한 도구를 미리 배달, 나는 그냥 쓰기만 하면 됨!

### 예시 (비교)
```java
// 기존 방식 (직접 만들기)
HelloService service = new HelloService();

// IoC 방식 (Spring이 미리 만들어줌 → 우리는 그냥 받아쓰기만)
@Autowired
HelloService service;
```

✔ 즉, **제어권이 개발자 → 프레임워크(SPRING)** 로 "역전".

---

##  2. DI (Dependency Injection, 의존성 주입)

###  IoC를 실현하는 방법 중 하나.

**"내가 필요로 하는 객체(의존성)를 Spring이 대신 주입해주는 방식"**  
→ 개발자가 일일이 `new HelloService()` 하지 않아도 된다!

###  예시 코드

```java
@Service
public class HelloService {
    public String greet(String name) {
        return "Hello " + name;
    }
}

@Controller
public class HelloController {
    private final HelloService helloService;

    // 생성자 주입 방식 (Spring이 HelloService를 대신 넣어줌)
    public HelloController(HelloService helloService) {
        this.helloService = helloService;
    }

    @GetMapping("/hello")
    @ResponseBody
    public String hello(@RequestParam String name) {
        return helloService.greet(name);
    }
}
```

 포인트:  
- HelloController는 HelloService가 필요하지만, 직접 만들지 않아도 됨.
- Spring이 알아서 HelloService 객체를 만들어서 **"주입(inject)"**!

---

##  3. AOP (관점 지향 프로그래밍) – Aspect Oriented Programming

###  이건 뭐냐면…
> "공통 기능을 핵심 로직과 **분리해서 따로 관리**하자!"는 개념.

###  왜 필요할까?
모든 클래스에 **반복되는 코드(로깅, 보안, 트랜잭션 등)** 가 계속 들어가면,
코드가 지저분해짐! 

그걸 분리해서 깔끔하게 관리하자는 게 AOP.

###  예시 상황:
- 모든 함수 실행 전에 **로그를 출력하고 싶다**
- 모든 DB 작업 후 **트랜잭션을 커밋하고 싶다**

### 💡 해결책 = AOP

###  코드 예시

```java
@Aspect // 이 클래스는 AOP에서 사용하는 "관점 클래스"
@Component
public class LoggingAspect {

    // 어떤 함수에 적용할지 지정 (Controller 패키지 내 모든 메서드 실행 전)
    @Before("execution(* com.example.controller.*.*(..))")
    public void logBefore() {
        System.out.println("메서드 실행 전 로그 출력!");
    }

    @After("execution(* com.example.controller.*.*(..))")
    public void logAfter() {
        System.out.println("메서드 실행 후 로그 출력!");
    }
}
```

###  사용되는 어노테이션 요약

| 어노테이션 | 설명 |
|------------|------|
| `@Aspect` | 이 클래스는 AOP에 사용됨 |
| `@Before` | 특정 함수 실행 전에 작동 |
| `@After` | 함수 실행 후에 작동 |
| `@Pointcut` | 어떤 함수에 적용할지 조건 설정 |

---

##  AOP 적용 결과

→ 우리는 **HelloController**에서 **로깅 코드**를 따로 쓰지 않아도,  
AOP가 자동으로 처리해준다!  
→  코드가 훨씬 깔끔해지고 유지보수도 쉬워진다!

---

##  마무리 요약

| 개념 | 핵심 요약 |
|------|-------------|
| IoC | Spring이 객체 생성/제어를 대신함 |
| DI | 필요한 객체를 Spring이 대신 넣어줌 |
| AOP | 공통 기능(로깅 등)을 코드 밖에서 처리하는 방식 |

---

