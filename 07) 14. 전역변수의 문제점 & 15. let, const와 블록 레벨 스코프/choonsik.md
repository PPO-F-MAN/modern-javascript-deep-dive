# 14장 전역 변수의 문제점

## 변수의 생명 주기

### 지역 변수의 생명 주기

**변수**는 `선언`에 의해 생성되고 `할당`을 통해 값을 갖는다.
변수는 생성되고 소멸되는 `생명 주기`가 있다.

변수는 자신이 선언된 위치에서 생성되고 소멸한다.
그러므로 전역 변수의 생명 주기는 애플리케이션의 생명 주기와 같다.

함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸된다.

```js
function foo() {
  var x = "local";
  console.log(x); // local
  return x;
}
foo();
console.log(x); // ReferenceError: x is not defined
```

지역 변수 `x`는 foo 함수가 호출되면서 생성되고, foo 함수가 종료될 때 소멸되어 생명 주기가 종료된다.
즉,

> **함수 내부에서 선언된 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.**

**변수**

1. 하나의 값을 저장하기 위해 확보한 메모리 공간 자체
2. 그 메모리 공간을 식별하기 위해 붙인 이름

**변수의 생명 주기**
그 메모리 공간이 **확보**된 시점부터
메모리 공간이 **해제**되어 가용 메모리 풀에 반환되는 시점까지이다.

함수 내부에서 선언된 **지역 변수**는,

- 함수가 생성한 스코프에 등록된다.

  - 함수가 생성한 스코프는 렉시컬 환경이라 부르는 물리적 실체가 있다.

- **변수는 자신이 등록된 스코프가 소멸(메모리 해제)될 때까지 유효하다.**
  - 할당된 메모리 공간은 더 이상 누구도 참조하지 않을 때 가비지 콜렉터에 의해 해제되어 가용 메모리 풀에 반환된다. (스코프도 마찬가지)

```js
var x = "global";

function foo() {
  console.log(x); // undefined
  var x = "local";
}
foo();
console.log(x); // global
```

> 호이스팅은 스코프를 단위로 동작한다.

### 전역 변수의 생명 주기

> **전역 변수의 생명 주기** === **전역 객체의 생명 주기**

- **전역 코드**

  - 명시적인 호출이나 특별한 진입점 없이 곧바로 해석되고 시작된다.
  - 더 이상 행할 문이 없을 때 종료된다.

- **전역 객체**
  - 코드 실행 이전에 어떤 객체보다 먼저 생성되는 특수한 객체
  - 클라이언트 사이드 환경의 `window`, 서버 사이드 환경의 `global` 객체를 의미
    > 전역 객체를 가리키는 식별자로 `globalThis`를 쓰자는 내용이 명세에 추가되었다.
  - 표준 빌트인 객체(`Object`, `String`, `Number`, `Function`, `Array` ...)와 환경에 따른 호스트 객체(클라이언트 Web API, Node.js의 호스트 API), var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

> **var 키워드로 선언한 전역 변수의 생명 주기 === 전역 객체의 생명 주기**

- var 키워드로 선언한 전역 변수는 전역 객체 `window`의 프로퍼티이다.
- 전역 객체 `window`는 웹페이지를 닫기 전까지 유효하다.
- 브라우저 환경에서 var 키워드로 선언한 전역 변수는 웹페이지를 닫을 때까지 유효하다.

## 전역 변수의 문제점

### 암묵적 결합

- 모든 코드가 전역 변수를 참조하고 변경할 수 있는 **암묵적 결합**을 허용한다.
  - 변수의 유효 범위가 커지면서 코드의 가독성은 나빠지고,
  - 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다.

### 긴 생명 주기

- 전역 변수는 생명 주기가 길다.

  - 즉, 메모리 리소스를 오랜 시간 소비한다.

- 특히 var 키워드는 변수의 `중복 선언`을 허용하므로
  - 생명 주기가 긴 전역 변수는 변수 이름이 중복될 수 있고
  - 변수 이름이 중복되면 의도치 않은 재할당이 이뤄진다.

### 스코프 체인 상에서 종점에 존재

- 변수를 검색할 때 가장 마지막에 검색된다. 즉 검색 속도가 가장 느리다.

### 네임스페이스 오염

- 자바스크립트는 파일이 분리되어 있어도 하나의 전역 스코프를 공유한다.
  - 다른 파일에서 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.

## 전역 변수의 사용을 억제하는 방법

> **변수의 스코프는 좁을수록 좋다.**
> 전역 변수를 반드시 사용해야 할 이유가 없다면 지역 변수를 사용해야 한다.

### 즉시 실행 함수

> 모든 코드를 즉시 실행 함수로 감싸서 모든 변수는 즉시 실행 함수의 지역 변수로 만든다.

선언과 동시에 초기화되므로 플러그인, 라이브러리를 만들 때 많이 사용하며
모듈 패턴의 기반이 되기도 한다.

```js
(function () {
	var foo = 10; // 즉시 실행 함수의 지역 변수
    ...
}());

console.log(foo); // ReferenceError: foo is not defined
```

### 네임스페이스 객체

> 전역에 네임스페이스 역할을 담당할 객체를 생성하고, 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가한다.

이 방법은 식별자 충돌을 방지할 수는 있으나, 어쨌든 네임스페이스 객체 자체는 전역 변수에 할당된다.

```js
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = "Lee";

console.log(MYAPP.name); // Lee

MYAPP.person = {
  name: "Lee",
  address: "Seoul",
};

console.log(MYAPP.person.name); // Lee
```

```js
// a.js
var MYAPP = MYAPP || {};
MYAPP.name = "Lee";

// b.js
var MYAPP = MYAPP || {};
MYAPP.name = "Lee";
```

### 모듈 패턴

> 클래스를 모방해서 관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다.
> 모듈 패턴은 자바스크립트의 `클로저`를 기반으로 동작하며,
> 전역 변수 억제는 물론 `캡슐화`까지 구현할 수 있다.

#### 캡슐화

`프로퍼티`와 `메서드`를 하나로 묶는 것

`프로퍼티` : 객체의 상태
`메서드` : 프로퍼티를 참조하고 조작할 수 있는 동작

#### 정보 은닉

자바스크립트는 `public`, `private`, `protected` 등의 접근 제한자를 허용하지 않는다.
자바스크립트에서는 객체의 특정 프로퍼티나 메서드를 감추기 위해 `모듈 패턴`을 사용한다.

```js
// 객체를 반환하는 즉시 실행 함수
var Counter = (function () {
  // private 변수
  var num = 0;

  // public 멤버 (외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체)
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

### ES6 모듈

> ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

ES6 모듈 내에서 var 키워드로 선언한 변수는 전역 변수도, window 객체의 프로퍼티도 아니다.
script 태그에 `type="module"`을 추가하면 파일은 모듈로서 동작하며, 모듈 파일 확장자는 `mjs`를 권장한다.

```js
<script type="module" src="app.mjs></script>
```

ES6 모듈은 구형 브라우저에서 동작할 수 없으며, 트랜스파일링이나 번들링이 필요하기 때문에 아직까지는 Webpack 등의 모듈 번들러를 사용하는 것이 일반적이다.

# 15장 let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

### 변수 중복 선언 허용

```js
var x = 1;
var y = 1;

var x = 100;
var y; // 초기화문이 없는 변수 선언문은 무시된다.

console.log(x); // 100
console.log(y); // 1
```

위처럼 초기화문의 유무에 따라 다르게 동작한다.

### 함수 레벨 스코프

> var 키워드로 선언한 변수는 **함수의 코드 블록**만을 **지역 스코프**로 인정한다.
> 즉, 함수 외부에서 var 키워드로 선언한 변수는 코드 블록(조건문, 반복문 등) 내에서 선언해도 모두 전역 변수가 된다.

```js
var x = 1;

if (true) {
  var x = 10;
}

console.log(x); // 10

var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}
console.log(i); // 5
```

### 변수 호이스팅

> var 키워드에 의한 변수 호이스팅은 프로그램의 흐름상 맞지 않고 가독성을 떨어뜨리며 오류를 발생할 여지를 남긴다.

```js
console.log(foo); // undefined

foo = 123;

console.log(foo); // 123

var foo;
```

## let 키워드

### 변수 중복 선언 금지

> let이나 const 키워드로 선언된 변수는 같은 스코프 내 중복 선언을 허용하지 않는다.

```js
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 블록 레벨 스코프

> let 키워드로 선언한 변수는 **모든 코드 블록**(함수, 조건문, 반복문, try/catch 문 등)을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따른다.

```js
let foo = 1;
{
  let foo = 2;
  let bar = 3;
}
console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

함수도 코드 블록이므로 스코프를 만든다.
이 때, 함수 내의 코드 블록은 함수 레벨 스코프에 중첩된다.

```js
let i = 10;
function foo() {
  let i = 100;
  for (let i = 1; i < 3; i++) {
    console.log(i); // 1, 2
  }
  console.log(i); // 100
}
foo();
console.log(i); // 10
```

### 변수 호이스팅

> var 키워드와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```js
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

var 키워드 선언의 경우, **런타임 이전에** 자바스크립트 엔진에 의해 암묵적으로 **선언 단계와 초기화 단계**가 진행되었다.

1. 선언 단계에서 스코프(실행 컨텍스트의 렉시컬 환경)에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알리고,
2. 초기화 단계에서 `undefined`로 변수를 초기화했다.

let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.

1. 런타임 이전에 암묵적으로 선언 단계만 먼저 실행되고
2. 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

#### 일시적 사각지대(Temporal Dead Zone, TDZ)

스코프의 시작지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간

```js
console.log(foo); // ReferenceError. TDZ

let foo;
console.log(foo); // undefined

foo = 1;
console.log(foo); // 1
```

이 때문에 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보이지만..

```js
let foo = 1; // 전역 변수
{
  console.log(foo); // ReferenceError: TDZ
  let foo = 2; // 지역 변수
}
```

> 자바스크립트는 모든 선언(var, let, const, function, function\*, class 등)을 호이스팅하지만, let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

### 전역 객체와 let

var 키워드로 선언한 전역 변수와 전역 함수,
그리고 선언하지 않은 변수에 값을 할당한 `암묵적 전역`은 **전역 객체 window의 프로퍼티**가 된다.
(전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다)

```js
// 브라우저 환경

var x = 1;
y = 2; // 암묵적 전역

function foo() {}

console.log(window.x); // 1
console.log(x); // 1
console.log(window.y); // 2
console.log(y); // 2

console.log(window.foo); // f foo() {}
console.log(foo); // f foo() {}
```

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니며 위와 같이 접근할 수 없다.
let 전역 변수는 보이지 않는 개념적 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.

```js
//브라우저 환경
let x = 1;

console.log(window.x); // undefined
console.log(x); // 1
```

## const 키워드

> const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지 않는다.

### 선언과 초기화

> const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

```js
const foo; // SyntaxError: Missing initializer in const declaration
const foo = 1;
```

const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```js
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

console.log(foo); // ReferenceError: foo is not defined
```

### 재할당 금지

```js
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 상수

> const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다.
> 이러한 특징을 이용해 const 키워드를 `상수`를 표현하는데 사용하기도 한다.

`상수`도 값을 저장하기 위한 메모리 공간이 필요하므로 **재할당이 금지된 변수**라고 말할 수 있다.

`상수`는 **상태 유지**와 **가독성**, **유지보수의 편의**를 위해 적극적으로 사용해야 한다.
일반적으로 상수는 대문자로 선언하며, 스네이크 케이스로 표현한다.

```js
const TAX_RATE = 0.1;

let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```

### const 키워드와 객체

> const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
> 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통한 객체 변경이 가능하다.

```js
const person = {
  name: "Lee",
};

person.name = "Kim";

console.log(person); // {name: "Kim"}
```

이 때, 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

## var vs let vs const

변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우 한정적으로 사용하는 것이 좋다.

- **ES6를 사용한다면 var 키워드는 사용하지 않는다.**
- 재할당이 필요한 경우에 한정해 let 키워드를 사용하고, 이때 변수의 스코프는 최대한 좁게 만든다.
- 읽기 전용 원시 값과 객체에는 const 키워드를 사용한다.
