# MyBatis vs JPA 

##  1. 배경 지식

###  -> 자바로 DB에서 데이터 가져오고 싶음
> "User 테이블에서 id가 1인 사람을 가져와서 화면에 보여줘"

하지만...
- 자바는 객체(Object)를 다룸
- DB는 테이블(Table), SQL을 다룸

이 두 세계는 말이 다름 → **언어의 차이**
→ 이걸 연결해주는 것이 **JDBC**, 그리고 그 위에 만들어진 **MyBatis, JPA**

---

##  2. JDBC - 가장 원시적이고 기본적인 방법

###  개념
- JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 연결하기 위한 가장 기초적인 API
- MyBatis와 JPA의 기반 기술이기도 하며, 직접 사용 시 복잡한 코드 작성이 필요할 수 있다.
- 직접 SQL을 작성하고 실행하며, 결과도 직접 처리

### 🔧 상세 코드 예시
```java
// 데이터베이스 연결 (Connection 객체 생성)
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "pass");

// SQL 쿼리 준비 (PreparedStatement 사용하여 SQL Injection 방지)
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
stmt.setInt(1, 1); // 쿼리에 id 값 지정

// SQL 실행 및 결과 저장
ResultSet rs = stmt.executeQuery();

// 결과 처리
if(rs.next()){
    System.out.println(rs.getString("name"));
}

// 자원 정리 (반드시 필요)
rs.close();
stmt.close();
conn.close();
```


### **JDBC의 한계와 Persistence Framework의 등장:**

* **JDBC의 복잡성:**
    * **보일러플레이트 코드**: 반복적이고 비효율적인 코드
    * JDBC는 데이터베이스와의 직접적인 상호작용을 위해 필요한 많은 코드를 요구함.
    * 데이터베이스 연결, SQL 실행, 결과 처리, 예외 처리 등 복잡한 과정을 개발자가 직접 관리해야 한다.
    * 이는 코드의 가독성을 떨어뜨리고 개발 생산성을 저하시키는 요인으로 작용함.
* **개발자의 부담 증가:**
    * JDBC 코드는 오류 발생 가능성이 높고, 유지보수가 어렵다.
    * 특히, 데이터베이스 연결과 같은 자원 관리는 개발자의 세심한 주의를 필요로 한다.
    * 이러한 부담을 줄이기 위해 **Persistence Framework**가 도입됨.
* **Persistence Framework의 역할:**
    * Persistence Framework는 JDBC의 복잡한 과정을 추상화하여 개발자가 데이터베이스 작업을 보다 쉽고 효율적으로 수행할 수 있도록 지원한다.
    * 데이터베이스 연결, SQL 실행, 결과 처리 등을 자동화하여 개발자는 비즈니스 로직에 집중할 수 있다.
    * **Persistence Framework는 내부적으로는 JDBC를 사용하여 DB에 접근하여 개발자가 직접 JDBC를 사용하지 않을 뿐 내부적으로는 JDBC를 통해 DB에 접근한다**.

**Persistence Framework의 분류:**

* **SQL Mapper:**
    * SQL 문을 직접 작성하고, 그 결과를 객체에 매핑하는 방식.
    * SQL에 대한 높은 제어력을 제공하며, 복잡한 쿼리나 성능 최적화가 필요한 경우에 유용하다.
    * **대표적인 SQL Mapper로는 MyBatis가 있다.**
* **ORM (Object-Relational Mapping):**
    * 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑하는 방식.
    * 객체 지향 프로그래밍과 데이터베이스 간의 불일치를 해결하고, 개발 생산성을 향상시킨다.
    * **대표적인 ORM 프레임워크로 JPA(Java Persistence API), Hibernate가 있다**.

---

##  3. MyBatis와 SQL Mapper - "SQL은 내가 쓰고, 객체 변환만 맡길래"

#### SQL Mapper란?
* 개발자는 SQL만 작성하고, 객체 변환을 자동으로 도와주는 기술

####  MyBatis란?
* 대표적인 SQL Mapper 프레임워크
* XML이나 어노테이션으로 SQL 작성, 결과 객체화는 MyBatis가 처리

**작동 원리:**
* **SqlSessionFactory**: MyBatis 설정 관리 및 SqlSession 생성
* **SqlSession**: 실제 SQL 실행 및 트랜잭션 관리
* **Mapper 인터페이스**: SQL 쿼리와 Java 메서드 매핑
* **XML 매핑 파일**: SQL 쿼리 및 결과 매핑 정의
####  상세 코드 예시
#### XML 매퍼
```xml
<select id="getUser" resultType="User">
  SELECT * FROM users WHERE id = #{id}
</select>
```
#### Java 코드
```java
// MyBatis 매퍼 인터페이스를 통해 호출
User user = userMapper.getUser(1);
System.out.println(user.getName());
```
**장점**
> MyBatis는 SQL을 직접 작성하여 사용하기 때문에 복잡한 SQL 쿼리를 매우 세부적이고 명확하게 관리할 수 있다는 장점이 있다. 
또한 동적 쿼리 작성이 간편하여 조건에 따라 다양한 쿼리를 쉽게 구현할 수 있으며, 성능 최적화를 위한 세부적인 컨트롤도 가능하다.
> SQL 중심이어서 학습 난이도도 비교적 낮고, 기존의 레거시 시스템과의 통합도 용이하다는 점에서 실무 프로젝트에서 많이 사용된다.

**단점**
> 반면, MyBatis는 객체지향적이지 않으며 SQL 중심으로 작성되므로 데이터베이스 구조가 변경되면 코드 유지보수가 어렵고 번거로울 수 있다.
> 또한 반복적이고 단순한 CRUD 작업에서도 항상 SQL을 작성해야 하기 때문에 생산성 면에서 비효율적일 수 있다.
> 결국 SQL 의존적인 개발을 피할 수 없게 됨
---

##  4. JPA와 ORM - 객체 중심으로 데이터를 다루자

####  ORM(Object Relational Mapping)이란?
* 객체 지향 프로그래밍과 관계형 데이터베이스 간의 차이를 자동으로 연결하는 기술
* Java 클래스 ↔ DB 테이블 매핑

####  JPA(Java Persistence API)란?
* ORM 기술을 위한 표준 API
* 대표적인 구현체가 **Hibernate**
* 데이터베이스 테이블과 Java 객체(Entity)를 직접 연결하여 객체 단위로 데이터 관리
* 복잡한 SQL 구문을 직접적으로 다루는것을 피하고 객체 중심으로 개발하는데 적합

**작동 원리:**
* **EntityManager:** 영속성 컨텍스트(Persistence Context) 관리
* **JPQL(Java Persistence Query Language):** 객체 중심의 쿼리 언어
* **Criteria API:** 객체 중심의 동적 쿼리 생성 API
* **Entity 클래스:** 데이터베이스 테이블과 매핑되는 Java 객체

####  상세 코드 예시
```java
// EntityManager를 통해 객체 조회
User user = entityManager.find(User.class, 1); // 내부적으로 SELECT 자동 생성
System.out.println(user.getName());

// 새 객체 저장
entityManager.persist(new User("Alice")); // 내부적으로 INSERT 자동 생성
```

####  Hibernate란?
* JPA의 구현체, 내부적으로 SQL을 자동 생성하고 실행
* 캐싱, 지연 로딩(Lazy loading), 프록시(proxy) 기능 제공

#### Hibernate의 주요 특징

#### 1. 객체 중심적 설계
* 데이터베이스와 자바 객체 간의 자동 매핑 기능 제공
* 비즈니스 로직과 데이터 접근 로직의 명확한 분리

#### 2. 자동화된 CRUD 연산
* 기본적인 CRUD(Create, Read, Update, Delete) 작업을 자동으로 처리하여 생산성 향상

#### 3. DB 독립성
* Hibernate의 Dialect 설정을 통해 다양한 데이터베이스(MySQL, Oracle, PostgreSQL 등)를 쉽게 지원

#### 4. 성능 최적화 기능
* 1차 캐시(영속성 컨텍스트 수준), 2차 캐시(애플리케이션 수준) 지원
* Lazy Loading 및 Fetch 전략 제공

#### 5. 쿼리 작성의 유연성
* JPQL(Java Persistence Query Language) 및 Criteria API, 네이티브 SQL 지원으로 다양한 쿼리 작성 가능

---

## 5. MyBatis vs JPA 

| 항목 | MyBatis | JPA |
|------|---------|-----|
| SQL 작성 | 직접 작성 | 자동 생성 (JPQL/HQL) |
| SQL 자유도 | 높음 | 제한적 |
| 객체 중심 설계 | 약함 | 강함 |
| 코드 양 | SQL이 많음 | 코드가 간결 |
| 성능 튜닝 | 수동으로 쉬움 | 내부 동작을 이해해야 가능 |
| 복잡한 쿼리 | 우수 | JPQL 필요 (복잡성 증가) |
| 학습 난이도 | 상대적으로 낮음 | 높음 |
| 유지보수 | SQL 많아지면 복잡 | 구조적으로 명확 |

---

##  6. 활용 사례

**MyBatis**는 Legacy 시스템이나 복잡한 SQL 처리가 필수적인 **은행 시스템**과 같은 프로젝트에서 자주 사용된다. 
**JP**A는 빠른 개발과 객체 중심의 서비스 개발이 중요한 커머스 플랫폼 등에서 활용된다(단순 CRUD 중심 서비스).

두 기술 간 전환 시 MyBatis에서 JPA로 전환할 경우 객체 중심의 리팩토링이 필요하며, 반대로 JPA에서 MyBatis로 전환하면 세부 성능을 더 자세히 제어할 수 있다.

##  7. 구조 비교 시각화

```
[내 코드] → [JPA(Hibernate)] → [JDBC] → [DB]
```

```
[내 코드] → [MyBatis] → [JDBC] → [DB]
```

```
[내 코드] → [JDBC] → [DB]
```

---

##  8. 핵심 정리
- JDBC: 모든 DB 기술의 기초
- MyBatis: SQL 중심, 매핑 자동화
- JPA(Hibernate): 객체 중심, 자동 매핑
- 기술 선택은 "SQL을 얼마나 직접 다룰 것인지"에 따라 결정

---



