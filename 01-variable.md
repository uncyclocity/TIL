# 01. 변수의 선언 및 할당

> References <br> <a href="http://www.yes24.com/Product/Goods/92742567?OzSrank=1">모던 자바스크립트 Deep Dive</a> _.이웅모_ <br> <a href="https://github.com/FE-Lex-Kim/-TIL/blob/master/Javascript/변수 및 동작원리 이해.md">변수 및 동작원리 이해</a> _.FE-Lex-Kim_ <br> <a href="https://github.com/FE-Lex-Kim/-TIL/blob/master/Javascript/호이스팅.md">호이스팅</a> _.FE-Lex-Kim_ <br> <a href="https://github.com/FE-Lex-Kim/-TIL/blob/master/Javascript/스코프.md">스코프</a> _.FE-Lex-Kim_

## 1. 선언과 할당

```javascript
var num; // 선언
num = 1; // 할당
```

- ### 선언

  - 메모리 주소에 붙인 이름을 **식별자**라고 하며, 변수명은 이에 해당한다.
  - 식별자를 JS 엔진에 알리는 것을 **선언**이라고 한다.
  - 변수 선언 ➡️ 메모리 공간 확보 ➡️ 식별자와 메모리 주소 연결 ➡️ 값 저장을 위한 준비 순으로 진행된다.

- ### 할당
  - 변수에 값을 저장하는 것을 **할당**이라고 한다.
  - 변수에 새로운 값을 다시 할당하는 것을 **재할당**이라고 하며, 이때 **새로운 메모리 공간**을 확보한 뒤 할당한다. <br>
    👉 이때 어떠한 식별자와도 연결되어 있지 않은 **쓰레기 값**이 되며, 주기적으로 작동되는 **가비지 콜렉터**에 의해 메모리 공간이 해제된다.
  - 변수 선언 시 `undefined`로 초기화가 진행되는데, 메모리 공간 내부에 다른 앱에서 사용되었던 쓰레기 값이 있을 수 있기 때문이다. <br>
    👉 그렇기에 선언 이후 첫 할당 또한 사실상 재할당에 해당한다.

## 3. `var`, `let`, `const`의 차이

- `var`

  - ECMAScript5 키워드이다.
  - **함수 스코프**에 해당한다.

    ```javascript
    // 같은 함수 내에 존재하는 모든 변수는 함수 내의 어디서든지 호출이 가능하다.
    var a = "hello";

    if (a === "hello") {
      var b = "world";
    }

    console.log(b); // world
    ```

  - 호이스팅에 의해 소스코드 평가과정에서 **선언 및 초기화**가 진행된다.

    ```javascript
    console.log(a); // undefined
    var a = "hello";
    ```

- `let`, `const`

  - ECMAScript6에서 추가된 키워드이다.
  - `const`의 경우 값의 재할당이 불가능한 **상수**이다.
    ```javascript
    const a = "hello";
    a = "world"; // 에러
    ```
  - **블럭 스코프**에 해당한다.

    ```javascript
    // 같은 블럭 내에 존재하는 모든 변수는 블럭 내의 어디서든지 호출이 가능하나, 블럭 밖에서는 호출할 수 없다.
    let a = "hello";

    if (a === "hello") {
      let b = "world";
    }

    console.log(b); // 미정의 에러
    ```

  - 호이스팅에 의해 소스코드 평가과정에서 **선언**이 진행되고, 변수의 선언문에 도달할 때 **초기화**가 진행된다.
    ```javascript
    // 평소에는 호이스팅이 안되는 것처럼 보일 수도 있으나, 만약 안된다면 본 예제의 경우 콘솔에 "hello"가 찍혀야 한다.
    // 블록 내에서 지역변수 a이 선언되었으나 초기화가 되지 않았기에 참조 에러가 발생한다.
    let a = "hello";
    {
      console.log(a); // 참조 에러
      let a = "world";
    }
    ```
