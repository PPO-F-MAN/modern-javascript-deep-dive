# 26. ES6 함수의 추가 기능

## 1. 함수의 구분

ES6 이전의 모든 함수는 일반 함수로 호출(callable)할 수 있는 것은 물론 생성자 함수로도 호출(constructor)할 수 있다.

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --------------- | ----------- | --------- | ----- | --------- |
| 일반 함수       | O           | O         | X     | O         |
| 메서드          | X           | X         | O     | O         |
| 화살표 함수     | X           | X         | X     | X         |

## 2. 메서드

ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

```js
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty("prototype"); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty("prototype"); // -> true

String.prototype.toUpperCase.prototype; // -> undefined
String.fromCharCode.prototype; // -> undefined

Number.prototype.toFixed.prototype; // -> undefined
Number.isFinite.prototype; // -> undefined

Array.prototype.map.prototype; // -> undefined
Array.from.prototype; // -> undefined
```

ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.

```js
const base = {
  name: "Lee",
  sayHi() {
    return `Hi! ${this.name}`;
  },
};

// 1
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  },
};

// 2
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  },
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

## 3. 화살표 함수

### 화살표 함수 정의

```js
const arrow = (x, y) => { ... };

const arrow = x => { ... };

const arrow = () => { ... };

// concise body
const power = x => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = x => { return x ** 2; };

const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

화살표 함수도 일급 객체이므로 Array.prototype.map, Array.prototype, filter, Array.prototype.reduce 같은 고차 함수에 인수로 전달할 수 있다.

화살표 함수는 콜백 함수로 정의할 때 유용하다.

### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.
2. 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.

### this

화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.
이를 lexical this라 한다.

화살표 함수 자체의 this 바인딩은 존재하지 않고, 화살표 함수 내부에서 this를 참조하면 일반적인 식별자처럼 스코프 체인을 통해 상위 스코프에서 this를 탐색한다.

화살표 함수 자체의 this 바인딩이 없기 때문에
Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 this를 교체할 수 없다.

일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 객체 리터럴을 바인딩하고 프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정한다.

### super

함수 자체의 super 바인딩을 갖지 않는다.
super를 참조하면 상위 스코프의 super를 참조한다.

### arguments

함수 자체의 arguments 바인딩을 갖지 않는다.
auguments를 참조하면 상위 스코프의 arguments를 참조한다.

## 4. Rest 파라미터

### 기본 문법

함수에 전달된 인수들의 목록을 배열로 전달받는다.

```js
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

rest파라미터는 반드시 마지막 파라미터야 한다.
Rest 파라미터는 단 하나만 선언할 수 있고, 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

### Rest 파라미터와 arguments 객체

arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며 함수 내부에서 지역 변수처럼 사용할 수 있다.

Rest 파라미터를 사용하면 유사 배열 객체인 arguments를 배열로 변환해야 하는 번거로움을 피할 수 있다.

화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.

## 5. 매개변수 기본값

인수가 전달되지 않은 매개변수의 값은 undefined다.
ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```js
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```

Rest 파라미터에는 기본값을 지정할 수 없다.

매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 아무 영향을 주지 않는다.
