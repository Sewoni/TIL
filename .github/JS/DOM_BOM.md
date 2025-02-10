
###  DOM (Document Object Model)란?
DOM은 웹 문서(HTML)의 구조를 **트리 형태로 표현한 객체 모델**임.  
브라우저가 HTML을 읽어 DOM을 생성하고, JavaScript를 통해 요소를 조작할 수 있음.  

👉 **DOM 트리 구조 예시**
```html
<html>
  <head>
    <title>DOM 예제</title>
  </head>
  <body>
    <h1 id="title">Hello, DOM!</h1>
    <button onclick="changeTitle()">Change Title</button>
  </body>
</html>
```
👉 **JavaScript를 이용한 DOM 조작**
```javascript
function changeTitle() {
    const title = document.querySelector("#title")
    title.innerText = "Hello, JavaScript & DOM!"
}
```

#### **DOM 관련 주요 메서드**  
| 메서드 | 설명 |
|--------|----------------|
| `document.getElementById("id")` | 특정 `id`를 가진 요소 선택 |
| `document.querySelector("css-selector")` | 첫 번째 일치하는 요소 선택 |
| `document.querySelectorAll("css-selector")` | 모든 일치하는 요소 선택 (NodeList 반환) |
| `element.innerHTML = "새로운 내용"` | HTML 변경 |
| `element.innerText = "새로운 텍스트"` | 텍스트 변경 |
| `element.style.color = "red"` | CSS 스타일 변경 |

---


### 🔍 **DOM vs BOM 차이점**  

#### **📌 1️⃣ DOM (Document Object Model)**
- **웹 페이지(HTML)의 구조를 트리 형태로 표현**한 객체 모델
- HTML 요소를 **조작**하고, **추가/삭제/수정**하는 데 사용됨
- `document` 객체를 통해 접근 가능  
- DOM을 사용하면 **HTML 태그, 텍스트, 속성 등을 변경 가능**  

👉 **예제: DOM 조작**
```html
<h1 id="title">Hello, DOM!</h1>
<button onclick="changeText()">Change Text</button>

<script>
    function changeText() {
        document.getElementById("title").innerText = "DOM이 변경됨!";
    }
</script>
```
🔹 **결과:** 버튼을 클릭하면 `<h1>` 태그의 내용이 변경됨.

---

#### **📌 2️⃣ BOM (Browser Object Model)**
- **브라우저의 기능을 조작하는 객체 모델**
- 브라우저 창, 히스토리, 네트워크 상태 등을 **제어**
- `window` 객체를 통해 접근 가능  
- DOM이 HTML 요소를 다루는 것과 달리, BOM은 **브라우저 자체를 다룸**  
- 브라우저에 따라 지원하는 기능이 다를 수 있음  

👉 **BOM의 주요 객체**  
| 객체 | 설명 |
|------|-----------------------------|
| `window` | 브라우저 창 전체를 의미 (최상위 객체) |
| `navigator` | 브라우저 정보 (ex. `navigator.userAgent`) |
| `location` | 현재 URL 정보 (ex. `location.href`) |
| `history` | 방문 기록 정보 (ex. `history.back()`) |
| `screen` | 사용자의 화면 크기 정보 (ex. `screen.width`) |

👉 **예제: BOM 조작**
```javascript
// 현재 페이지의 URL을 출력
console.log(window.location.href);

// 3초 후 새로운 페이지로 이동
setTimeout(() => {
    window.location.href = "https://www.google.com";
}, 3000);
```
🔹 **결과:** 3초 후 구글 페이지로 이동함.

---

### 🚀 **DOM vs BOM 비교 정리**
| 비교 항목 | DOM (문서 객체 모델) | BOM (브라우저 객체 모델) |
|----------|-----------------|-----------------|
| **정의** | 웹 문서(HTML)를 객체로 다루는 모델 | 브라우저 창과 관련된 기능을 다루는 모델 |
| **최상위 객체** | `document` | `window` |
| **역할** | HTML 요소를 조작 (추가, 수정, 삭제) | 브라우저 동작을 제어 (이동, 크기 조절, 히스토리 관리 등) |
| **예제** | `document.getElementById("title")` | `window.location.href = "https://..."` |
| **사용 예시** | 웹 페이지 내 버튼 클릭 시 글자 변경 | 사용자가 특정 시간 후 페이지 이동 |

---

### **📌 결론**
✔ **DOM**: HTML 문서를 조작하는 데 사용됨 (ex. 버튼 클릭 시 텍스트 변경)  
✔ **BOM**: 브라우저 자체를 조작하는 데 사용됨 (ex. 새 창 열기, URL 이동)  

👉 **한마디로 요약하면?**  
- **DOM = HTML 조작**  
- **BOM = 브라우저 조작**  

