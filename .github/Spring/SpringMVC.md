

## Spring MVC란?

**Spring MVC는 웹사이트(웹 애플리케이션)를 만들기 위한 설계 구조**

사람이 웹페이지에서 버튼을 누르거나 요청을 보냈을 때,  
컴퓨터가 그 요청을 처리하고, 알맞은 화면을 보여주는 흐름을 관리하는 시스템이다.

MVC는 **Model(모델)**, **View(화면)**, **Controller(제어자)** 로 구성.

---

##  MVC 구성요소 설명

### 1. Controller (컨트롤러)
- "누가 무엇을 눌렀는지" 알아챔.
- 사용자의 요청을 받아서 처리할 곳(서비스)을 연결해주고, 결과를 전달.

### 2. Model (모델)
- 실제 **데이터**를 담는 가방 같은 역할.
- Controller가 데이터를 여기에 넣어주고, View가 이 데이터를 꺼내서 보여줌.

### 3. View (뷰)
- 사용자에게 보여지는 화면이다.
- HTML, JSP, Thymeleaf 같은 기술로 만들어짐.

---

##  Spring MVC가 작동하는 순서

1. 사용자가 웹사이트에서 주소를 입력하거나 버튼 클릭
2. **DispatcherServlet**이라는 시스템이 요청을 받음
3. 이 요청을 **Controller**에게 전달함
4. Controller는 필요한 **Service (기능 처리)** 를 호출
5. 결과 데이터를 **Model**에 담아서
6. 어떤 **View(화면)** 를 보여줄지 정하고
7. View는 Model에 담긴 데이터를 꺼내 화면을 만듬
8. 사용자에게 그 결과를 보여줌

---

##  쉽게 비유하면?

- **Controller = 웨이터 (손님 주문 받음)**
- **Service = 주방 (요리함)**
- **Model = 접시 (요리 담김)**
- **View = 서빙 (손님에게 보여줌)**

---

##  코드 예제로 보기

### 1. Controller - 요청을 받는 부분
```java
@Controller // 이 클래스는 사용자의 요청을 처리하는 컨트롤러라고 알려주는 표시
public class HelloController {

    @GetMapping("/hello") // 사용자가 "/hello"라는 주소를 요청하면 이 함수를 실행하라는 뜻
    public String hello(Model model) {
        // Model은 데이터를 담는 그릇.
        // 여기서 "message"라는 이름으로 "안녕하세요!"를 담아 View로 보낸다.
        model.addAttribute("message", "안녕하세요!");

        // "hello"는 보여줄 화면 파일의 이름 (hello.html 또는 hello.jsp 등)
        return "hello";
    }
}
```

### 2. View - 사용자에게 보여주는 화면 (예: hello.html)

```html
<!DOCTYPE html>
<html>
<body>
  <!-- Controller에서 보낸 데이터 "message"를 꺼내서 화면에 보여줌 -->
  <h1 th:text="${message}">기본 메세지</h1>
</body>
</html>
```

※ `${message}`는 Controller에서 보낸 "안녕하세요!" 메시지를 꺼내는 표현.

---

### 3. Service - 실제 일을 처리하는 클래스 (선택사항)

```java
@Service // 이 클래스는 비즈니스 로직을 처리하는 서비스
public class HelloService {
    public String getGreeting() {
        return "안녕하세요! 서비스에서 가져온 인사입니다!";
    }
}
```

→ Controller에서 `HelloService`를 불러서 데이터를 가져올 수도 있다.

---

### 4. Controller + Service 조합

```java
@Controller
public class HelloController {

    // Service를 자동으로 연결해줌
    @Autowired
    private HelloService helloService;

    @GetMapping("/hello")
    public String hello(Model model) {
        // Service에서 인사 메시지를 가져옴
        String msg = helloService.getGreeting();
        model.addAttribute("message", msg);
        return "hello"; // View로 이동
    }
}
```

---

##  Spring MVC에서 중요한 기능들 

- `@Controller` → 요청을 받는 역할
- `@Service` → 실제 로직 처리
- `@Repository` → DB 연결할 때 사용
- `@GetMapping`, `@PostMapping` → 어떤 주소로 요청할 때 실행할 함수인지 지정
- `Model` → 데이터를 View로 전달하는 그릇
- `@RestController` → 화면 없이 JSON으로 응답 (API 만들 때 사용)

---

## 👍 Spring MVC 장점

1. 코드가 역할별로 나뉘어 깔끔함 (Controller/Service/Repository)
2. 유지보수하기 쉬움 (고치기 편함)
3. 확장하기 쉬움 (더 많은 기능 추가 가능)
4. 다양한 기술과 잘 어울림 (Spring Boot, JPA 등)

---

## 👎 단점

1. 처음 배우면 구조가 복잡해 보일 수 있음
2. 설정할 게 많아서 초반에 조금 어렵게 느껴질 수 있음

---

##  정리 한 줄

> **Spring MVC는 웹사이트에서 어떤 일이 일어날지를 잘 나누어서 관리할 수 있게 해주는 시스템이다.**

---

