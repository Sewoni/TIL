### **코딩에서 많이 사용되는 네이밍 컨벤션**  
코드를 읽기 쉽게 하고 일관성을 유지하기 위해 다양한 네이밍 규칙이 사용됩니다. 대표적으로 **카멜 표기법(camelCase)**, **케밥 표기법(kebab-case)**, **파스칼 표기법(PascalCase)**, **스네이크 표기법(snake_case)** 등이 있습니다.

---

## **1. 카멜 표기법 (camelCase)**
- **특징:** 단어들이 붙어 있으며, 첫 단어는 소문자로 시작하고 이후 단어의 첫 글자는 대문자로 시작함.
- **예제:**  
  - `userName`
  - `totalAmount`
  - `isLoggedIn`
- **사용되는 곳:**  
  - JavaScript, Java, C++, Swift 등의 변수 및 함수 이름
  - 예: `function getUserInfo() {}`

✅ **장점:**  
- 가독성이 좋고, 변수와 함수명을 짧게 유지할 수 있음.  
- 언어적인 흐름과 잘 맞아 자연스럽게 읽을 수 있음.  

---

## **2. 케밥 표기법 (kebab-case)**
- **특징:** 모든 단어를 소문자로 작성하고, 각 단어를 `-`(하이픈)으로 연결함.
- **예제:**  
  - `user-name`
  - `total-amount`
  - `is-logged-in`
- **사용되는 곳:**  
  - HTML, CSS의 클래스 및 ID 이름  
    ```css
    .main-container {
      background-color: #fff;
    }
    ```
  - 일부 URL 구조에서도 사용됨  
    ```
    https://example.com/user-profile
    ```

✅ **장점:**  
- HTML/CSS에서 주로 사용되며, 여러 단어를 구분할 때 직관적으로 보이게 함.  
- URL에 사용될 때 가독성이 높아짐.  

🚨 **주의할 점:**  
- JavaScript 변수나 함수명에서는 사용할 수 없음(변수명에 `-`가 들어가면 연산자로 인식됨).  

---

## **3. 파스칼 표기법 (PascalCase)**
- **특징:** 단어들의 첫 글자를 모두 대문자로 작성하며, 단어 사이의 공백은 사용하지 않음.
- **예제:**  
  - `UserName`
  - `TotalAmount`
  - `IsLoggedIn`
- **사용되는 곳:**  
  - Java, C#, TypeScript 등의 **클래스 이름**  
    ```java
    class UserProfile {
      // ...
    }
    ```
  - React의 **컴포넌트 이름**  
    ```tsx
    function UserProfile() {
      return <div>Hello</div>;
    }
    ```

✅ **장점:**  
- 클래스나 생성자 함수 등을 구별하는 데 유용함.  
- 코드의 일관성을 유지할 수 있음.  

🚨 **주의할 점:**  
- 변수나 함수명에 사용되지 않고, 주로 **클래스, 인터페이스, 타입 이름**에 사용됨.  

---

## **4. 스네이크 표기법 (snake_case)**
- **특징:** 모든 단어를 소문자로 작성하고, `_`(언더스코어)로 단어를 구분함.
- **예제:**  
  - `user_name`
  - `total_amount`
  - `is_logged_in`
- **사용되는 곳:**  
  - **Python 변수 및 함수명**  
    ```python
    def get_user_info():
        return "User Info"
    ```
  - **데이터베이스 필드 이름**  
    ```sql
    SELECT user_name FROM users;
    ```

✅ **장점:**  
- 언더스코어로 단어를 구분하기 때문에 가독성이 뛰어남.  
- 데이터베이스 및 Python 코드 스타일 가이드(PEP 8)에서 권장하는 방식.  

🚨 **주의할 점:**  
- JavaScript, Java, C++ 등의 언어에서는 변수명으로 잘 사용되지 않음.  

---

## **비교 정리**
| 표기법  | 예제 | 특징 | 주로 사용되는 곳 |
|---------|----------------|--------------------------------|-------------------|
| **camelCase** | `userName` | 첫 단어는 소문자, 이후 단어는 대문자 | JS, Java 변수 및 함수 |
| **kebab-case** | `user-name` | 소문자 단어를 `-`로 연결 | HTML, CSS 클래스 및 ID, URL |
| **PascalCase** | `UserName` | 모든 단어의 첫 글자가 대문자 | 클래스, 컴포넌트 이름 |
| **snake_case** | `user_name` | 단어를 `_`로 연결, 모두 소문자 | Python, 데이터베이스 |

📌 **TIP:**  
- **변수, 함수명 → `camelCase`**  
- **클래스, 컴포넌트 → `PascalCase`**  
- **HTML/CSS, URL → `kebab-case`**  
- **데이터베이스, Python 변수/함수 → `snake_case`**  

