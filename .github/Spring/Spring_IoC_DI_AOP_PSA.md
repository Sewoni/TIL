# POJO(Plain Old Java Object)란? 
**"옛날 방식의 간단한 자바 오브젝트"**
![spring](https://github.com/user-attachments/assets/fa9c7bfd-5f46-4c81-94d0-ce95bbae3de6)

위 이미지는 Spring 삼각형, Spring 의 핵심 개념들을 모두 표현하고 있음. 

**POJO: 필드의 Getter, Setter 와 같은 기본 기능만을 갖는 기본 객체를 의미.**    
**순수한 자바객체. 특정 라이브러리 환경, 프레임워크에 종속 x**    

> Spring 은 왜 탄생하였는가?
> > Spring이 POJO개념을 잘 지킨 프레임워크이기 때문이다.
> > > Java EE 등의 중량 프레임워크들의 등장으로, 해당 프레임워크에 종속된 무거운 객체를 만들게 된 것에 반발해서 사용됨! IOC, DI, AOP 와 같은 개념들이 모두 결합력을 느슨하게 하여 의존성을 낮춤으로써 종속성이 낮아지는 현상이 발생한다.
> > > > Spring의 철학 = POJO 기반 프로그래밍, Spring은 POJO를 Bean으로 등록해서 관리하고 주입함 → Spring Container가 POJO를 조립해서 애플리케이션 구성

**POJO를 쓰는 이유? (왜 중요할까?)**
- **기술 독립성**: 어떤 프레임워크에도 종속되지 않아 자유롭게 사용 가능
- **재사용성**: 다양한 환경, 프레임워크에서도 재사용 가능
- **유지보수 용이성**: 프레임워크 교체 시 코드 변경 최소화
- **단위 테스트 쉬움**: 의존성 없이 객체 단독 테스트 가능
- **가독성 / 단순성**: 코드 구조 단순화, 학습 비용 낮춤

**즉, “POJO로 작성된 코드는 유연하고 테스트가 쉽고 유지보수가 좋다 → 소프트웨어 품질 ↑”**

**POJO 규칙(특징)**
- **클래스는 일반 자바 클래스**: 어떤 라이브러리/프레임워크도 상속하지 않음
- **특정 인터페이스 구현 X**: Serializable, Runnable 같은 것도 필수 아님
- **애너테이션 없음**: 프레임워크 종속적인 어노테이션 없음 (@Entity, @Service 등)
- **접근자 메서드**: 보통 private 필드 + public getter/setter 형태
- **new 키워드로 생성 가능**: 별도 컨테이너 없이 직접 생성해서 사용 가능
  
예시)
```java
// POJO 클래스 - 아무런 어노테이션, 상속, 구현 없음
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter/Setter
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
// extends, implements 없음
// @Component, @Entity 같은 어노테이션 없음
// 단순히 생성자, 필드, getter/setter로 구성된 자바 객체

```
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
public class A {
b= new B();
}

// IoC 방식 (Spring이 미리 만들어줌 → 우리는 그냥 받아쓰기만)
public class A {
private B b; //코드에서 객체 생성 x, 받아온 객체를 b에 할당
}
```

```java
// 기존 방식 (직접 만들기)
HelloService service = new HelloService();

// IoC 방식 (Spring이 미리 만들어줌 → 우리는 그냥 받아쓰기만)
@Autowired
HelloService service;
```
✔ 즉, **제어권이 개발자 → 프레임워크(SPRING)** 로 "역전".    
-> 객체의 생명 주기 관리를 외부에 위임한다.    
-> 여기서 '외부(=객체를 관리하고 관리하는 주체)' 를 Spring Container/IoC Container 라고도 함 !

---


##  2. DI (Dependency Injection, 의존성 주입)

###  IoC를 실현하는 방법 중 하나.

**"내가 필요로 하는 객체(의존성)를 Spring이 대신 주입해주는 방식"**  
→ 개발자가 일일이 `new HelloService()` 하지 않아도 된다!    
→ 스프링에서는 의존성 주입으로 직접 객체를 생성하지 않고 제어권을 스프링 컨테이너로 넘긴다!

**의존성 주입 방법**
- **생성자 주입**: 가장 권장되는 방식 (불변성 보장, 테스트 쉬움)
- **필드 주입**: 간결하지만 테스트 어려움
- **세터 주입**: 선택적 의존성에 유리

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
### 빈(Bean) 이란? 

> **Spring Container에 의해 생성되고 관리되는 객체**를 **Bean**이라고 함.

즉, 우리가 직접 `new`로 만드는 게 아니라, **Spring이 만들어서 관리해주는 객체 = Bean** .

#### 왜 Bean이라는 용어를 쓸까?
- 과거 자바에서 **재사용 가능한 컴포넌트 객체**를 **JavaBean**이라고 부른 데서 유래.
- Spring도 **재사용 가능한 객체**들을 **컨테이너에서 관리하므로 “Bean”** 이라고 부름.


####  Bean은 누가 만들고 어디서 관리돼?
- **Spring IoC 컨테이너**가 Bean을 생성하고, DI를 통해 다른 객체에 주입까지 해줌.
- 대표적인 컨테이너: `ApplicationContext`

#### 어떻게 Bean으로 등록할까?

#### 방법 1️⃣: 어노테이션 방식 (가장 많이 씀)
```java
@Component // Bean 등록
public class HelloService {
    public String sayHello() {
        return "Hello!";
    }
}
```

#### 관련 어노테이션들
| 어노테이션 | 역할 |
|------------|----------------------------|
| `@Component` | 일반 컴포넌트 등록 |
| `@Service` | 서비스 역할 클래스 등록 |
| `@Repository` | DAO/DB 관련 클래스 등록 |
| `@Controller` | 웹 컨트롤러 등록 |

#### 방법 2️⃣: 직접 설정파일로 등록 (`@Bean`)
```java
@Configuration
public class AppConfig {
    
    @Bean
    public HelloService helloService() {
        return new HelloService();
    }
}
```

---

####  Bean의 특징

| 특징 | 설명 |
|--------|-----------------------------|
| Spring이 생성 | 개발자가 직접 new 안 함 |
| DI 대상이 됨 | 필요한 곳에 주입 가능 |
| 생명주기 관리 | 생성 ~ 소멸까지 Spring이 관리 |
| Scope 존재 | Singleton, Prototype 등 다양한 범위 설정 가능 |

---

#### Bean Scope (빈의 범위)

| Scope | 설명 |
|-------|-----------------------------|
| Singleton | (기본값) 애플리케이션 내 1개 인스턴스 공유 |
| Prototype | 요청마다 새로운 객체 생성 |
| Request | 웹 요청마다 1개 객체 (웹 컨텍스트 필요) |
| Session | 웹 세션마다 1개 객체 |

```java
@Component
@Scope("prototype")
public class MyBean {
    ...
}
```

---

#### Bean과 POJO의 관계는?

- **POJO 객체**를 Spring에 등록하면 → **Bean이 됨**
- Bean 자체는 특별한 객체가 아니고, **“Spring이 관리 중인 POJO”** 라고 보면 됨.

즉:
> “**모든 Bean은 POJO이지만, 모든 POJO가 Bean은 아니다**”

---

####  요약

| 항목 | 설명 |
|------|-------------------------------|
| Bean | Spring이 생성하고 관리하는 객체 |
| 등록 방식 | @Component, @Bean 등 |
| 용도 | IoC/DI 구조에서 연결 대상 |
| 범위 | Scope 설정 가능 (싱글톤 등) |
| 관계 | POJO → Spring이 관리 → Bean |

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
-> 공통 관심사항과 핵심 관심사항을 분리하기 위해 사용하는 것

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

-> @Transactional → 대표적인 AOP 기반 기능

---

###  AOP 적용 결과

→ 우리는 **HelloController**에서 **로깅 코드**를 따로 쓰지 않아도,  
AOP가 자동으로 처리해준다!  
→  코드가 훨씬 깔끔해지고 유지보수도 쉬워진다!

---

##  4. PSA (Portable Service Abstraction)란?

####  "간편한 서비스 추상화" 
> **Spring이 다양한 기술/서비스에 대한 "공통 추상화 계층"을 제공함으로써**,  
> **구현 기술이 바뀌어도 코드 변경 없이** 사용할 수 있도록 해주는 설계 철학.

스프링에서 데이터베이스에 접근하기 위한 기술로는 JPA, MyBatis, JDBC같은 것들이 있다. 
개발자는 **"JDBC를 쓰든 JPA를 쓰든, ActiveMQ를 쓰든 RabbitMQ를 쓰든"**  
**"Spring이 제공하는 통일된 인터페이스/API로만 프로그래밍"** 하면 된다는 것.

####  Spring에서 PSA가 왜 중요한가?

| 기존 방식 | PSA 방식 |
|------------|----------------|
| 각 기술에 따라 코드 바뀜 | 기술이 바뀌어도 코드 거의 동일 |
| JDBC → 코드 변경 필요 | Spring JDBC → 동일 인터페이스 사용 |
| 트랜잭션 처리 직접 구현 | Spring 트랜잭션 추상화 사용 |

➡ 즉, PSA는 **기술 종속성 최소화 → 유지보수성과 이식성 향상**  
➡ Spring의 **"확장성과 유연성"** 의 핵심 기반 중 하나


#### PSA가 적용된 대표 사례 in Spring

| 영역 | PSA 인터페이스 | 대표 구현체 예시 |
|------|----------------|--------------------|
| 트랜잭션 | `PlatformTransactionManager` | `DataSourceTransactionManager`, `JpaTransactionManager` |
| 데이터 접근 | `JdbcTemplate`, `JpaRepository` | JDBC, JPA, MyBatis |
| 메시지 처리 | `JmsTemplate`, `KafkaTemplate` | ActiveMQ, RabbitMQ, Kafka |
| 파일 업로드 | `MultipartResolver` | Commons, Servlet API |
| 캐시 | `CacheManager` | EhCache, Redis, Caffeine |


####  PSA 예시 1: 트랜잭션 관리

```java
@Transactional
public void doBusinessLogic() {
    // 내부적으로 어떤 DB 기술을 쓰든
    // Spring이 PlatformTransactionManager로 처리
}
```

- 우리는 `@Transactional`만 붙이면 끝.
- 내부에서는 Spring이 상황에 맞는 트랜잭션 매니저를 자동 적용:
  - JDBC: `DataSourceTransactionManager`
  - JPA: `JpaTransactionManager`

➡ **트랜잭션 기술이 바뀌어도 개발 코드는 그대로**


####  PSA 예시 2: 데이터 접근 – JdbcTemplate

```java
// 개발자는 SQL만 신경 씀
jdbcTemplate.query("SELECT * FROM users", resultSet -> {
    // 결과 처리
});
```

- **SQL 실행/커넥션 관리/예외처리 등은 Spring이 처리**
- 내부에서는 JDBC, 하지만 나중에 JPA나 MyBatis로 바꿔도 PSA 덕분에 구조 거의 동일


#### PSA 예시 3: 메시지 처리 – JmsTemplate

```java
jmsTemplate.convertAndSend("queueName", "Hello World");
```

- 개발자는 `jmsTemplate`만 사용  
- ActiveMQ든 RabbitMQ든 내부 구현이 바뀌어도 상관없음

####  PSA 구조 흐름 이해도

```
[개발자 코드]
   ↓
[Spring 추상화 계층 (PSA 인터페이스)]
   ↓
[실제 구현체 (JDBC, JPA, Kafka 등)]
```

➡ PSA 덕분에 **Spring 애플리케이션은 구현 기술이 아니라 “설계와 구조”에 집중 가능**

---

#### PSA의 장점 요약

| 장점 | 설명 |
|--------|----------------------------|
| 기술 독립성 | 기술이 바뀌어도 코드 영향 최소 |
| 일관된 코드 스타일 | 서비스 간 동일한 프로그래밍 모델 |
| 유지보수성 향상 | 교체 비용 낮고 학습 비용도 낮음 |
| 테스트 용이성 | PSA 인터페이스 기반 → Mock 테스트 쉬움 |

---

#### PSA는 결국 Spring의 철학

```
POJO 기반 설계
→ IoC/DI로 객체 연결
→ AOP로 공통기능 분리
→ PSA로 기술 추상화
```

###  마무리 요약

| 개념 | 핵심 요약 |
|------|-------------|
| IoC | Spring이 객체 생성/제어를 대신함 |
| DI | 필요한 객체를 Spring이 대신 넣어줌 |
| AOP | 공통 기능(로깅 등)을 코드 밖에서 처리하는 방식 |
| PSA | 다양한 기술을 통일된 방식으로 추상화 |

---

