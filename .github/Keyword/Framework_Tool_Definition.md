## Jest 🃏 (JavaScript 테스팅 프레임워크)
#### 📌 주 사용 분야: React, Node.js, TypeScript
  
 #### 📌 특징: 
  💡 설정이 거의 필요 없음</br>
  💡 빠르고 직관적인 API 제공</br>
  💡 스냅샷 테스트 지원</br>

  ```javascript
  test('adds 1 + 2 to equal 3', () => {
  expect(1 + 2).toBe(3);
});
```

  
## Unit Test 🔍 (단위 테스트)
#### 📌 개념:
 💡소프트웨어의 가장 작은 단위(함수, 클래스) 를 개별적으로 검증하는 테스트</br>
 💡코드 변경 시 빠르게 오류 감지 가능</br>
 💡Jest, Mocha, JUnit, PyTest 등 다양한 도구로 수행 가능</br>

 ```javascript
function add(a, b) {
  return a + b;
}

test('add function works correctly', () => {
  expect(add(2, 3)).toBe(5);
});
```

## ☕ Mocha (Node.js 기반 테스트 프레임워크)
#### 📌 주 사용 분야: Node.js 백엔드
#### 📌 특징:
  💡 BDD(Behavior-Driven Development) 스타일 지원</br>
  💡 Jest보다 유연한 설정 가능</br>
  💡 Chai와 함께 사용하면 더욱 강력한 테스트 가능</br>

  ```javascript
const { expect } = require('chai');

function add(a, b) {
  return a + b;
}

describe('Addition Function', () => {
  it('should return 5 for 2 + 3', () => {
    expect(add(2, 3)).to.equal(5);
  });
});
```

## 🚀 Postman (API 테스트 & 자동화 도구)
#### 📌 주 사용 분야: REST API, GraphQL API 테스트
#### 📌 특징:

💡 UI 기반으로 쉽게 API 요청 가능</br>
💡 API 자동화 테스트 기능 제공</br>
💡 API 문서화 및 Mock Server 기능 지원</br>


📝 활용 예시:

GET 요청: https://api.example.com/users</br>
POST 요청: JSON 데이터를 전송하여 API 테스트</br>


## 📊 **비교 및 추천**  

| 🚀 도구 | 🎯 용도 | 🔥 특징 | 🎯 추천 대상 |
|------|------|------|------------|
| 🃏 Jest | 단위 테스트, 통합 테스트 | 설정이 적고 빠름 | React, Node.js, TypeScript |
| ☕ Mocha | 단위 테스트, BDD 스타일 | 설정이 필요하지만 유연함 | Node.js 백엔드 |
| 🔍 Unit Test | 소프트웨어 테스트 기법 | 특정 프레임워크가 아니라 개념 | 모든 개발 환경 |
| 🚀 Postman | API 테스트 및 문서화 | UI 기반 API 테스트 가능 | 백엔드 개발, API 테스팅 |

🎯 **추천 조합:**  
- **프론트엔드 및 백엔드 단위 테스트** → Jest 🃏  
- **Node.js 백엔드 프로젝트** → Mocha ☕ + Chai  
- **API 테스트** → Postman 🚀  




