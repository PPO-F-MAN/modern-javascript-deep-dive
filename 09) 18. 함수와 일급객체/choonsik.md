# 함수와 일급객체

## 일급 객체

> 일급 객체란
>
> 1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
> 2. 변수나 자료구조에 저장할 수 있다.
> 3. 함수의 매개변수에 전달할 수 있다.
> 4. 함수의 반환값으로 사용할 수 있다.

자바스크립트의 함수는 일급 객체다.
=> 함수형 프로그래밍이 가능

## 함수 객체의 프로퍼티

`console.dir`
`Object.getOwnPropertyDescriptors()`

함수 객체 고유의 프로퍼티를 알아보자

### arguments 프로퍼티

> 함수 객체의 arguments 프로퍼티 값은 arguments 객체
> **arguments 객체** : 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체

자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.

callee 프로퍼티 : arguments 객체를 생성한 함수 (자기 자신)을 가리킨다.
length 프로퍼티 : 인수의 개수를 가리킨다.
Symbol(Symbol.iterator) 프로퍼티 : arguments 객체를 순회 가능한 자료구조인 이터러블로 만들기 위한 프로퍼티

```js
function symboltest(x, y) {
  const iterator = arguments[Symbol.iterator]();
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: 2, done: false}
  console.log(iterator.next()); // {value: undefined, done: true}

  return x * y;
}
symboltest(1, 2);
```

- `arguments` 객체
  - 가변 인자 함수를 구현할 때 유용하다.
  - 유사 배열 객체이다. (length 프로퍼티를 가진 객체. for문으로 순회할 수 있는 객체. 배열 메서드는 불가능)

```js
function sum() {
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}
console.log(sum(1, 2)); // 3
```

```js
function sum(args...) {
  return args.redece((pre, cur) => pre + cur, 0);
}
```

### caller 프로퍼티

> 함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

```js
function foo(func) {
  return func();
}

function bar() {
  return "caller : " + bar.caller;
}

console.log(foo(bar)); // caller : function foo(func) {...}
console.log(bar()); // caller : null
```

### length 프로퍼티

> 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

arguments 객체의 length : 인자의 개수
함수 객체의 length : 매개 변수의 개수

### name 프로퍼티

> 함수 이름을 나타낸다.

```js
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

var anonymousFunc = function () {};
// ES5 : name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6 : name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

function bar() {}
console.log(bar.nae); // bar
```

### `__proto__` 접근자 프로퍼티

> 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이것은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

`__proto__` 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 간접적으로 접근하기 위해 사용하는 **접근자 프로퍼티**다.

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체 Object.prototype 프로퍼티를 상속 받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```

### prototype 프로퍼티

> 생성자 함수로 호출할 수 있는 함수 객체, 즉 `constructor`만이 소유하는 프로퍼티

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('property'); / false
```

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때
생성자 함수가 생성할 인스턴스의 **프로토타입 객체**를 가리킨다.
