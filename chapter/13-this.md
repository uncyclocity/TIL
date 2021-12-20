# 13. this

## 1. this 키워드

- **this**란? 자신이 속한 객체 혹은 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**이다.
- 함수가 호출되면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
- this는 **함수 호출 방식**에 의해 동적으로 결정된다.

## 2. 함수 호출 방식에 따른 this 바인딩

```javascript
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: "bar" };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

1. **일반 함수 호출**

```javascript
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

- 일반함수로 호출되면 this는 **항상 전역객체를 참조**한다.
- 메서드 내부에 정의한 중첩 함수 또한 **일반 함수로 호출 시 전역 객체가 바인딩**된다. <br>
  👉 이때, **call/apply/bind**를 통해 다른 함수로 바인딩해주면 된다.

  ```javascript
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // 콜백 함수에 명시적으로 this를 바인딩한다.
      setTimeout(
        function () {
          console.log(this.value); // 100
        }.bind(this),
        100
      );
    },
  };

  obj.foo();
  ```

2. **메서드 호출**

```javascript
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee

const anotherPerson = {
  name: "Kim",
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim
```

- **메서드를 호출한 객체**에 바인딩된다.

3. **생성자 함수 호출**

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- **미래에 생성 될 인스턴스**가 바인딩된다.

## 3. Function.property.apply/call/bind 메서드에 의한 간접 호출

```javascript
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) {1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ}
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) {1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ}
// {a: 1}
console.log(getThisBinding.bind(thisArg)(1, 2, 3));
// Arguments(3) {1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ}
// {a: 1}
```

- **apply**는 인수를 배열로 전달한다.
- **call**은 인수를 쉼표로 구분하여 전달한다.
- **bind**는 기본적으로 함수를 호출하지 않으며, this로 사용할 객체만 전달한다. <br>
  👉 메서드 내부의 중첩함수나 콜백함수의 this가 메서드의 this와 일치하지 않는 문제를 해결하는 데 유용하게 사용된다.

  ```javascript
  const person = {
    name: "Lee",
    foo(callback) {
      // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
      setTimeout(callback.bind(this), 100);
    },
  };

  person.foo(function () {
    console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
  });
  ```
