# 18. for...in과 for...of의 차이

> References <br> <a href="https://velog.io/@eomttt/for-...in-for-...of-차이">for ...in, for ...of 차이</a> _.eomttt_

## 1. for...in

```javascript
var obj = {
  a: 1,
  b: 2,
  c: 3,
};

for (var item in obj) {
  console.log(item); // a, b, c
}
```

- **객체의 키를 순환**하는 반복문이다.
- 배열 또한 객체이기에, 키에 해당하는 **배열의 인덱스**값이 순서대로 출력된다.

  ```javascript
  var arr = [1, 2, 3];

  for (var item in arr) {
    console.log(item); // 0, 1, 2
  }
  ```

## 1. for...of

```javascript
var arr = [1, 2, 3];

for (var item of arr) {
  console.log(item); // 1, 2, 3
}
```

- **배열을 순환**하는 반복문이다.
- **객체는 사용이 불가능**하다.
