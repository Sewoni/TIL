### 📌 **CRUD란?**  
CRUD는 소프트웨어에서 **데이터를 다루는 기본적인 네 가지 기능**을 의미하는 약어입니다.  

| **기능** | **설명** | **SQL 연산** | **HTTP 메서드** |
|---------|--------|--------------|--------------|
| **C**reate | 데이터 생성 | `INSERT` | `POST` |
| **R**ead | 데이터 조회 | `SELECT` | `GET` |
| **U**pdate | 데이터 수정 | `UPDATE` | `PUT` 또는 `PATCH` |
| **D**elete | 데이터 삭제 | `DELETE` | `DELETE` |

---

### 🔹 **CRUD 상세 설명**  

1️⃣ **Create (생성)**  
   - 새로운 데이터를 추가하는 작업  
   - SQL: `INSERT INTO users (name, age) VALUES ('Alice', 25);`  
   - HTTP: `POST /users`  

2️⃣ **Read (조회)**  
   - 기존 데이터를 가져오는 작업  
   - SQL: `SELECT * FROM users WHERE id = 1;`  
   - HTTP: `GET /users/1`  

3️⃣ **Update (수정)**  
   - 기존 데이터를 변경하는 작업  
   - SQL: `UPDATE users SET age = 26 WHERE id = 1;`  
   - HTTP: `PUT /users/1` 또는 `PATCH /users/1`  

4️⃣ **Delete (삭제)**  
   - 데이터를 제거하는 작업  
   - SQL: `DELETE FROM users WHERE id = 1;`  
   - HTTP: `DELETE /users/1`  

---

### 🎯 **CRUD를 활용하는 예제 (JavaScript + Supabase)**  

#### 📌 **1. Create (사용자 추가)**
```javascript
const { data, error } = await supabase
  .from("users")
  .insert([{ name: "Alice", age: 25 }]);
```

#### 📌 **2. Read (사용자 조회)**
```javascript
const { data, error } = await supabase
  .from("users")
  .select("*")
  .eq("id", 1);
```

#### 📌 **3. Update (사용자 정보 수정)**
```javascript
const { data, error } = await supabase
  .from("users")
  .update({ age: 26 })
  .eq("id", 1);
```

#### 📌 **4. Delete (사용자 삭제)**
```javascript
const { data, error } = await supabase
  .from("users")
  .delete()
  .eq("id", 1);
```

---

### 🚀 **CRUD가 중요한 이유**
✔ 웹 애플리케이션의 기본적인 데이터 처리 방식  
✔ 데이터베이스, REST API, GraphQL 등에서 필수적인 개념  
✔ 사용자 관리, 게시판, 쇼핑몰, 블로그 등 **모든 서비스의 핵심 로직**  

