# 18.1 일급 객체

일급 객체는 다음과 같은 조건을 만족한다.

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
- 변수나 자료구조에 저장할 수 있다. (배열과 객체같은 곳)
- 함수의 매개변수에 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

그러니까 변수에 담을 수 있고, 반환 값으로 함수를 내뱉을 수도 있고, 어디서나 저장이 가능하면 일급 객체다.
자바스크립트의 함수는 위와 같이 사용할 수 있어서 **일급 객체**다.

# 18.2 함수 객체의 프로퍼티

함수는 객체다. 그래서 프로퍼티를 가질 수 있다.
`argumnets`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다. 일반 객체에는 없고, 함수에만 고유하게 있는 프로퍼티다.

`__proto__`는 단순히 접근자 프로퍼티이고, 함수 객체 고유의 프로퍼티가 아니다.
`__proto__`는 `Object.prototype` 객체의 프로퍼티를 상속받은 것을 알 수 있다. 
`Object.prototype` 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다.

## 18.2.1 arguments 프로퍼티

> 함수 객체의 arguments 프로퍼티 값은 arguments 객체이다. 해당 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열이다. 함수 내부에서 지역변수처럼 쓰이고, 외부에선 사용 불가능이다.

- 함수에 매개변수를 적게 전달해도, 많이 전달해도 자바스크립트는 무시하고 실행한다. (오류가 안 난다.)
- 초과된 인수는 그냥 버려지는 것은 아니고, `arguments` 객체의 프로퍼티로 보관된다.
- arguments는 배열이 아니라 **유사 배열 객체**라서 배열과 관련된 메소드를 사용하면 에러가 발생한다.

## 18.2.2 caller 프로퍼티

> caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다. 이후 표준화될 예정도 없는 프로퍼티이므로 사용하지 말고 참고로만 알아두자. 관심이 없으면 지나쳐라.

## 18.2.3 length 프로퍼티

> 함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

- arguments 객체의 length 프로퍼티는 인자(argument)의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개 변수(parameter)의 개수를 가리킨다.

```javascript
function add(parameter1, parameter2) {
	arguments.length // 3
}

add(argument1, argument2, argument3);

add.length // 2
```

## 18.2.4 name 프로퍼티

> 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다. ES6에 표준이 됐다.

- 익명 함수 표현식의 경우 ES5, ES6의 동작방식이 다르다.
- ES5에서는 name 프로퍼티는 빈 문자열을 값으로 갖는다.
- ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```javascript
const anonymousFunc = function() {};
// ES5: anonymousFunc.name = 빈 문자열
// ES6: anonymousFunc.name = anonymousFunc;
```

## 18.2.5 __proto__ 접근자 프로퍼티

> 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 갖는다. 저 객체는 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

- `__proto__` 는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.
- 내부 슬롯에는 직접 접근은 불가능하고, 던더 프로토를 통해서 간접적으로 접근할 수 있다.

## 18.2.6 prototype 프로퍼티

> prototype 프로퍼티는 생성자 함수로 호출할 수 있는 객체, 즉 constructor만이 소유하는 프로퍼티다.
일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.