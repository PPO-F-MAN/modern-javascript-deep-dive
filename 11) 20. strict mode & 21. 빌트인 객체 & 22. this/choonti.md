# strict mode

## strict mode란?

자바스크립트 언어의 문법을 좀 더 엄격히 적용해 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

ESLint 같은 린트 도구로도 유사한 효과를 얻을 수 있다.

## strict mode의 적용

```js
`use strict`;
```

위 코드를 사용하면 스크립트 전체에 strict mode가 적용된다.

## 전역에 strict mode를 적용하는 것은 피하자

strict와 아닌 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.
외부 서드파티 라이브버리를 사용하는 경우 라이브러리가 non-strict-mode일 수 있으므로 실행 함수의 선두에 strict mode를 적용하는 등으로 사용하자.

## 함수 단위로 strict mode를 적용하는 것도 피하자

모든 함수에 일일이 적용하는 것이나, 외부 컨텍스트에 적용되어 있지 않는 경우 문제가 될 수 있으므로 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## strict mode가 발생시키는 에러

### 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError

### 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError

### 매개변수 이름의 중복

중복된 매개변수 이름 SyntaxError

### with 문의 사용

SyntaxError. 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 성능과 가독성에 문제가 될 수 있다.

## strict mode 적용에 의한 변화

### 일반 함수의 this

함수를 일반 함수로 호출하면 this에 undefined가 바인딩된다.

### arguments 객체

매개변수에 전달된 인수를 재할당해도 arguments 객체에 반영되지 않는다.

# 빌트인 객체

## 자바스크립트 객체의 분류

- 표준 빌트인 객체 : 애플리케이션 전역의 공통 기능을 재공한다. 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.

- 호스트 객체 : 브라우저 환경에서는 DOM, BOM 등 클라이언트 사이드 Web API를 호스트 객체로 제공하고 Node.js 환경에서는 Node.js의 API를 호스트 객체로 제공한다.

- 사용자 정의 객체 : 사용자가 직접 정의한 객체

## 표준 빌트인 객체

Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 표준 빌트인 객체를 제공한다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다. 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

String, Number, Boolean, Function, Array, Date는 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.

## 원시값과 래퍼 객체

문자열, 슷자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라 한다.

## 전역 객체

코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며 어떤 객체에도 속하지 않은 최상위 객체

globalThis는 모든 환경에서 사용하는 전역 객체를 말한다.

전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

- 개발자가 의도적으로 생성할 수 없다. 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 안흔ㄴ다.
- 전역 객체의 프로퍼티를 참조할 때 window(global)를 생략할 수 있다.

- 전역객체는 모든 표준 빌트인 객체를 프로퍼티로 갖는다.

- 자바스크립트 실행환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.

- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.

- let, const 키워드 전역변수는 전역 객체의 프로퍼티가 아니다.

- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다.

## 빌트인 전역 프로퍼티

전역 객체의 프로퍼티를 말한다.
Infinity, NaN, undefined

## 빌트인 전역 함수

애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.
eval : 전달받은 문자열 코드가 표현식이라면 런타임에 평가하고, 아니라면 런타임에 실행한다. 기존의 스코프를 런타임에 동적으로 수정한다. 단 엄격모드에서는 자신의 자체적인 스코프를 생성한다. 보안과 최적화의 측면에서 사용을 금지하자

isFinite
isNaN
parseFloat
encodeURI/decodeURI는 URI의 문자들을 이스케이프 처리를 위해 인코딩/디코딩한다.
encodeURIComponent/decodeURIComponent URI 구성 요소를 인코딩/디코딩한다. 인수를 쿼리 스트링의 일부로 간주해 =,?,&까지 인코딩한다는 점에서 다르다.

## 암묵적 전역

선언하지 않은 식별자에 값을 할당해 전역 객체의 프로퍼티가 되면 전역 변수처럼 동작한다. 이것은 변수가 아니므로 변수 호이스팅이 발생하지 않는다. 전역 변수와 달리 delete 연산자로 삭제할 수 있다.

# this

## this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

메서드가 자신의 객체 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

재귀적인 참조는 바람직하지 않으므로, this라는 특수한 식별자가 존재한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며 코드 어디서든 참조할 수 있다. this가 가리키는 값, 즉 this의 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

this 바인딩 : 식별자와 값을 연결하는 과정

자바스크립트의 this 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.
일반적으로 객체의 메서드 내부, 생성자 함수 내부에서만 의미가 있다.

## 함수 호출 방식과 this 바인딩

this바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.

### 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다.
일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다.

### 메서드 호출

메서드 내부의 this는 메서드가 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

이들 메서드는 모든 함수가 사용할 수 있으며 첫 번째 인수로 전달한 객체가 this바인딩된다.

apply, call 메서드의 본질적인 기능은 함수를 호출하는 것이다.
bind 메서드는 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.
bint 메서드는 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.
