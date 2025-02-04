# 🥑 Javascript에서 Packing vs Unpacking

### 🌸 배열 패킹 (Packing)
```Javascript
const data = [1, 2, 3];  // 여러 개의 값을 하나의 배열에 저장
console.log(data);  // [1, 2, 3]
```
### 🌸 배열 패킹  (Unpacking, 구조 분해 할당 - Destructuring)
```Javascript
const [a, b, c] = data;
console.log(a);  // 1
console.log(b);  // 2
console.log(c);  // 3
```

### 🌸 **객체 패킹 & 언패킹**
```Javascript
const person = { name: "Alice", age: 25 };  // 패킹
const { name, age } = person;  // 언패킹
console.log(name);  // Alice
console.log(age);   // 25
```

### 🌸 **패킹(Packing) & 언패킹(Unpacking) 개념 정리**  

💡 **패킹** → 여러 개의 값을 **하나의 변수에 묶는 것**  
💡 **언패킹** → 묶여 있는 값을 **개별 변수로 풀어내는 것**  

---


## 🍀 **정리**  

| 개념 | 패킹 (Packing) | 언패킹 (Unpacking) |
|------|--------------|----------------|
| **설명** | 여러 개의 값을 하나의 변수에 저장 | 묶인 값을 개별 변수로 분해 |
| **예시 (JS 배열)** | `const data = [1, 2, 3];` | `const [a, b, c] = data;` |
| **예시 (JS 객체)** | `const obj = { x: 10, y: 20 };` | `const { x, y } = obj;` |

💡 **배열, 튜플, 객체에서 자주 활용되며, 구조 분해 할당(Destructuring) 개념과 연결됨!** 🚀
