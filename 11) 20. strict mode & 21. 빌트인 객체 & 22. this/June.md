# 20.1 strict mode란?

```javascript
function foo() {
  x = 10;
}

foo();

console.log(x); // ?
```

위의 예제는 참조 에러를 띄울 것 같지만, 자바스크립트의 암묵적 전역 현상에 의해서 전역 객체에 x가 들어가기 때문에 에러가 나지 않는다.

이런 전역 객체와 같은 유연함은 잠재적인 오류를 발생시킬 가능성이 있다.

`strict mode`는 이러한 유연한 자바스크립트의 문법을 조금 엄격하게 검사하고, 잠재적으로 에러를 일으킬 수 있는 코드에 명시적으로 에러를 발생시키는 것을 의미한다.

`ESLint`가 대체제가 될 수 있다.

```javascript
"use string";

function foo() {
  x = 10; // 참조 에러 x is not defined
}

foo();
```

```javascript
function foo() {
  x = 10; // 에러가 나지 않는다.
  ("use string");
}

foo();
```

`use strict`의 실행은 코드 순서에 영향을 받는다.

# 20.3 전역으로 strict mode를 적용하는 것은 피하자

> 전역에 적용한 strict mode는 스크립트 단위로 적용된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      "use strict";
    </script>
    <script>
      x = 1; // 에러가 나지 않는다.
    </script>
  </body>
</html>
```

`strict mode` 스크립트와 `non-strict mode` 스크립트를 혼용하는 건 좋지않다.
외부 라이브러리를 사용한다면 해당 라이브러리가 `non-strict mode` 일 경우가 있기 때문에
그런 경우에는 에러를 일으킬 수 있다.

```javascript
(function () {
  "use strict";

  // Do something
})();
```

# 20.4 함수단위로도 적용하지 말자.

함수단위로 strict mode를 적용할 수 있지만, 모든 함수에 일일이 strict mode를 적용하는 것은 고역이다.

오늘의 결론:

1. strict mode를 사용할거면 즉시실행함수로 감싼 스크립트 단위로 적용하자.
2. ESLint를 사용하자.

# 20.5 strict mode가 발생시키는 에러

## 20.5.1 암묵적 전역

아까 우리가 봤던 겁니다.

## 20.5.2 변수, 함수, 매개변수의 삭제

`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 에러가 뜹니다.

```javascript
(function () {
  "use strict";

  var x = 1;

  delete x; // 우리는 참된 자바스크립트인이니까 이런 것을 사용하지 않죠?
})();
```

## 20.5.3 매개변수 이름의 중복

```javascript
(function () {
  "use strict";

  function foo(x, x) {
    // 우리는 참된 자바스크립트인이니까 이런 것을 사용하지 않죠 2 ?
    return x + x;
  }
})();
```

## 20.5.4 with 문의 사용

> with 문은 전달된 객체를 스코프 체인에 추가한다. 성능과 가독성이 나빠지는 문제가 있어서 사용하지 않는다고 한다.

```javascript
(function () {
  "use strict";

  // 저는 못난 자바스크립트인이어서 있는지도 몰랐습니다
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

# 20.6 strict mode 적용에 의한 변화

## 20.6.1 일반 함수의 this

strict mode를 사용하면 일반 함수에서 this가 undefined가 된다고 합니다.

왜? 생성자 함수가 아니면 일반 함수 내부에서는 this를 사용할 필요가 없거든요

에러는 발생하지 않는다고 하네요

## 20.6.2 arguments 객체

```javascript
(function (a) {
  "use strict";

  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
})();
```

---

# 빌트인 객체

# 21.1 자바스크립트 객체의 분류

1. 표준 빌트인 객체: ECMAScript에 정의된 객체, 선언 없이 전역 변수처럼 언제나 사용할 수 있는 객체
2. 호스트 객체: ECMAScript에 정의되지 않은 객체, 브라우저에선 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 API를 호스트 객체로 제공, Node.Js에서는 Node.js 고유의 API를 호스트 객체로 제공한다.
3. 사용자 정의 객체: 우리가 직접 생성한 것

# 21.2 표준 빌트인 객체

> Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개의 표준 빌트인 객체 제공

- Math, Reflect, JSON을 제외하면 전부 인스턴스를 생성할 수 있다. (생성자 함수로 호출이 가능하다.)

- String 생성자 함수를 통해서 만든 String 인스턴스의 프로토타입은 String.prototype 이다.
- Number 생성자 함수를 통해서 만든 Number 인스턴스의 프로토타입은 Number.prototype 이다.
- Boolean 생성자 함수를 통해서 만든 Boolean 인스턴스의 프로토타입은 Boolean.prototype 이다.
- ...
- 이하 40개 동일

# 21.3 원시값과 래퍼 객체

> 문자열, 숫자, 불리언은 원시 값이 있는데 굳이 String, Number, Boolean과 같은 표준 빌트인 생성자 함수가 왜 필요하나?

```javascript
const str = "hello";

str.length;
str.toUpperCase();
```

자바스크립트 엔진이 암묵적으로 연관된 객체를 생성해서 생성된 객체로 프로퍼티에 접근하거나 메서드를 호춣하고 다시 원시값으로 되돌린다.

이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 "래퍼 객체" 라고 한다.

![래퍼 객체](./JuneImage/rapper.jpeg)
(~Rapper~) Wrapper 객체

래퍼 객체의 처리가 종료되면 원래 값으로 되돌리고 (래퍼 객체의 연결을 끊고), 래퍼 객체는 가비지 컬렉터의 대상이 된다.

# 21.4 전역 객체

> 전역 객체는 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체, 어떤 객체에도 속하지 않는 최상위 객체.

- 브라우저: window, self, this, frames
- 노드: global
- ES11 (에크마스크립트 11) 이후: globalThis

제일 최상위 객체? 그러면 프로토타입 상속 관계상에서 최상위 객체라는 의미?

- 그건 아니다. 어떤 객체의 프로퍼티도 아니며, 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 의미

전역 객체의 특징에는 아래와 같다.

- 개발자가 의도적으로 생성 불가, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체의 프로퍼티를 참조할 때 window, 혹은 global을 생략 가능
- 위에서 본 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 브라우저냐, Node.js환경이냐에 따라 추가적인 프로퍼티와 메서드를 갖는다.
  - 브라우저: DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker...
  - Node.js: Node.js 고유의 API
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
- let, const는 아니다. (보이지 않는 개념적 블록에 존재)
- 브라우저의 모든 자바스크립트 코드는 하나의 window를 공유한다.

## 21.4.1 빌트인 전역 프로퍼티

### Infinity

> 무한대를 나타낸다.

```javascript
console.log(window.Infinity === Infinity); // true

console.log(3 / 0); // Infinity
console.log(-3 / 0); // -Infinity
console.log(typeof Infinity); // number
```

### NaN

> Not a Number

```javascript
console.log(window.NaN); // NaN

console.log(Number("xyz")); // NaN
console.log(1 * "string"); // NaN
console.log(typeof NaN); // number
```

### undefined

> 정의되지 않음

```javascript
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

## 21.4.2 빌트인 전역 함수

### eval

> 자바스크립트 코드를 나타내는 문자열을 인수로 전달받고, 전달받은 문자열 코드가 표현식이면 문자열을 런타임에 평가해서 값을 생성하고, 표현식이 아니라 문이면 해당 문을 런타임에 실행한다.

```javascript
// 표현식인 문
eval("1 + 2;"); // 3

// 표현식이 아닌 문
eval("var x = 5;"); // undefined
```

`eval` 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하다, 또한 `eval` 함수를 통해 실행되는 코드는 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드 실행에 비해 처리 속도가 느리다. 따라서 쓰지말자.

### isFinite

> 전달받은 인수가 정상적인 유한수인지 검사하여 유한수면 `true`를 반환

```javascript
isFinite(0); // true
isFinite(2e64); // true
isFinite("10"); // true: '10' -> 10
```

### isNaN

> 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다.

```javascript
isNaN(NaN); // true
isNaN(10); // false
```

### parseFloat

> 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

```javascript
parseFloat("3.14"); // 3.14
parseFloat("10.00"); // 10

// 공백으로 나뉜 문자열은 맨 앞을 실수로 변경
parseFloat("34 45 67"); // 34
parseFloat("40 years"); // 40

// 맨 앞이 숫자로 변경 불가능하면 NaN
parseFloat("years 40"); // NaN

// 공백 제거
parseFloat(" 40 "); // 40
```

### parseInt

> 전달받은 문자열 인수를 정수로 해석하여 반환한다.

parseInt의 두번 째 인자는 진법을 나타내느 기수(2 ~ 36)을 전달할 수 있다.
기수를 지정하면 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석해서 반환한다.
이 때 반환값은 언제나 10진수다.

```javascript
parseInt("10"); // 10

// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다.
parseInt("10", 2); // 2

// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다.
parseInt("10", 8); // 8

// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다.
parseInt("10", 16); // 16
```

가끔 알고리즘 문제에서 진수변환 문제가 나오는데 그럴 땐
`parseInt`와 `toString`을 잘 섞어보자.

```javascript
const x = 15;

// 15를 2진수로 변환해서 문자열로 반환
x.toString(2); // '1111'

// 15를 8진수로 변환해서 문자열로 반환
x.toString(8); // '17'

// 15를 16진수로 변환해서 문자열로 반환
x.toString(16); // 'f'
```

첫 번째 인수로 전달한 문자열이 해당 지수의 숫자로 변경될 수 없으면 NaN을 반환한다.

```javascript
// 'A'는 10진수로 해석할 수 없다.
parseInt("A0"); // NaN

// '2'는 2진수로 해석할 수 없다.
parseInt("20", 2); // NaN
```

### encodeURI / decodeURI

> encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

`가`라는 문자가 URI에 들어가있으면, 어떤 시스템에서는 URI를 읽지 못할수도 있다.

- `www.example.com?name=가` // 인코딩 전
- `www.example.com?name=%EC%9E%90` // 인코딩 후

인코딩을 통해서 모든 시스템에서 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

- decodeURI는 당연히 인코딩된 것을 원래로 되돌리는 것이겠죵?

### encodeURIComponent / decodeURIComponent

위의 `encodeURI / decodeURI`와 비슷하다. 차이점은 아래와 같다.

- `encodeURIComponent`는 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주하기 때문에 쿼리 스트링 구분자로 사용되는 `=`, `?`, `&` 까지 인코딩해버린다.
- `encodeURI`는 매개변수로 전달된 문자열을 완전한 URI라고 생각하기 때문에 쿼리 스트링 구분자로 사용되는 `=`, `?`, `&`을 인코딩하지 않는다.

## 21.4.3 암묵적 전역

```javascript
var x = 10;

function foo() {
  // 선언하지 않은 y
  y = 20; // window.y = 20
}

foo();

// 전역 변수처럼 y를 사용할 수 있다.
// y는 암묵적 전역이다.
console.log(x + y); // 30
```

암묵적 전역으로 생성된 프로퍼티는 전역 변수처럼 동작하지만, y는 변수가 아니라 단지 전역 객체의 프로퍼티로 추가된 것 뿐이다. 그래서 호이스팅이 되지 않는다.

```javascript
console.log(x); // undefined

// 호이스팅이 일어나지 않는다.
console.log(y); // 참조 에러

var x = 10;

function foo() {
  y = 20; // window.y = 20
}

foo();

// 전역 변수처럼 y를 사용할 수 있다.
// y는 암묵적 전역이다.
console.log(x + y); // 30
```
