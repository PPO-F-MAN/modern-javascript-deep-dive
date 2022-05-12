# 18. 함수와 일급객체

## 18.1 일급객체

> **일급객체 조건**

1. 무명의 리터럴로 생성 가능. 즉, 런타임에 생성 가능
2. 변수나 자료구조(객체, 배열)에 저장 가능
3. 함수의 매개변수에 전달 가능
4. 함수의 반환값으로 사용 가능



자바스크립트에서의 함수는 위의 조건을 모두 만족하므로 일급 객체다.

```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function(num){
  return ++num;
};

const decrease = function(num){
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const predicates = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(predicate){
  let num = 0;

  return function(){
    num = predicate(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2
```

함수는 일급 객체이므로 **객체와 동일하게 사용**할 수 있으며, 값을 사용할 수 있는 곳이라면 **어디서든지 리터럴로 정의**할 수 있고, **런타임에 함수 객체로 평가**됨

일급 객체로서 함수가 가지는 가장 큰 특징은 **일반 객체와 같이 함수의 매개변수에 전달**할 수 있으며, **함수의 반환값으로 사용 가능**하다는 것

함수는 객체이지만 일반 객체와 다르게 **호출할 수 있고**, 일반 객체에는 없는 **함수 고유의 프로퍼티를 소유**함



## 18.2 함수 객체의 프로퍼티

> 함수는 객체이므로 프로퍼티를 가진다.

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.png?raw=true" align="left" width=800px/>

* arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티 (일반 객체에는 없음)

* `__proto__` 는 object.prototytpe 객체의 프로퍼티를 상속받은 접근자 프로퍼티

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2-1.png?raw=true" align="left" width="700px"/>



### 18.2.1 arguments 프로퍼티

**arguments 객체** : 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체. 함수 외부에서는 참조할 수 없음

함수 객체의 arguments 프로퍼티 값은 arguments 객체인데, **arguments 프로퍼티는 ES3부터 표준에서 폐지됨**

따라서, Function.arguments와 같은 사용법은 권장되지 않으며, 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체 참조



자바스크립트에서는 함수의 매개변수와 인수의 갯수가 일치하는지 확인하지 않음 => 에러 발생 X

* 선언된 매개변수의 개수보다 인수가 적게 전달되면, 전달되지 않은 매개변수는 undefined 상태 유지

* 선언된 매개변수의 개수보다 인수가 많이 전달되면, 초과된 인수는 무시 (버려지지는 않고 arguments 객체의 프로퍼티로 보관)

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.1.png?raw=true" align="left" width="800px"/>

<br>                                                                                                                                                                                                           

**arguments 객체**

> * 인수 : 프로퍼티 값, 인수의 순서 : 프로퍼티 키
>
> * callee 프로퍼티 : 호출되어 arguments 객체를 생성한 함수. 즉, 함수 자신
>
> * length 프로퍼티 : 인수의 개수
> * Symbol(Symbol.iterator) 프로퍼티 : arguments 객체를 iterable 자료구조로 만들기 위한 프로퍼티 (34장 이터러블 참조)



arguments 객체는 유사배열객체(array-like object)로 for문으로 순회할 수 있지만(iterable), 배열 메서드를 사용할 수는 없다.

따라서 배열 메서드를 사용하려면 Function.prototype.call, Function.prototype.apply를 사용하여 간접 호출해야 한다. (22.2.4절 참조)

```js
function sum() {
  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (x, y) {
    return x + y;
  }, 0);
}

console.log(sum(1, 2)); 			// 3
console.log(sum(1, 2, 3, 4)); // 10
```

ES6에서는 Rest 파라미터 도입 (파라미터 앞에 ... 붙이면 배열로 받음) (26.4절 Rest 파라미터 참조)

```js
// ES6 Rest parameter
function sum(...args) {
  return args.reduce((x, y) => x + y, 0);
}

console.log(sum(1, 2)); 			// 3
console.log(sum(1, 2, 3, 4)); // 10
```



### 18.2.2 caller 프로퍼티

> 함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

ECMAScpit 사양에 포함되지 않은 **비표준화 프로퍼티**로 이후에도 표준화될 예정이 없으므로 사용하지 말자.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/caller

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.2-1.png?raw=true" align="left" width="400px"/>

위의 예제에서 함수 호출 bar()를 호출한 함수는 없기 때문에 null 출력

node 에서는 module 때문에 결과가 달라짐 (48장 모듈 참조)<br>

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.2-2.png?raw=true" align="left"/>



### 18.2.3 length 프로퍼티

> 함수를 정의할 때 선언한 매개변수의 개수

- arguments 객체의 length 프로퍼티 : 인수(argument)의 개수

- 함수 객체의 length 프로퍼티 : 매개변수(parameter)의 개수

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.3.png?raw=true" align="left" width="300px"/>



### 18.2.4 name 프로퍼티

> 함수 객체의 name 프로퍼티는 함수 이름을 나타냄

name 프로퍼티는 ES6 이전까지는 비표준이었다가 ES6에서 정식 표준이 됨

익명 함수 표현식의 name 프로퍼티는 ES5에서 빈 문자열을 값으로 갖지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖음

```js
// 기명 함수 표현식
var namedFunc = function foo(){};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function(){};
// ES5: name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6: name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문(Function declaration)
function bar(){}
console.log(bar.name); // bar
```



### 18.2.5 `__proto__` 접근자 프로퍼티

모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가지는데, 이는 프로토타입 객체를 가리킨다.

`__proto__` 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 **접근자 프로퍼티**

내부 슬롯에는 접근할 수 없어서 [[Prototype]] 내부 슬롯에도 접근할 수 없고, `__proto__` 접근자 프로퍼티를 통해 간접적으로 접근 가능

```js
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype
console.log(obj.__proto__ === Object.prototype); // true

// 객체 레터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype 의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype 의 메서드다.
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```



참고) [[Prototype]] 변경하는 거 느리고 다른 객체들에게 영향을 미칠 수 있어서 비추. Object.create() 로 원하는 프로토타입 가지고 있는 새 객체 생성하는 거 권장드림. 

프로토타입 반환하게 하려면 `__proto__` 대신 Object.getPrototypeOf(), 교체하고 싶은 경우에 `Object.SetprototypeOf` 메서드를 사용할 것을 권장(19장 참조)

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/proto 



### 18.2.6 prototype 프로퍼티

생성자 함수로 호출할 수 있는 함수 객체. 즉, constructor 만이 소유하는 프로퍼티. 일반 객체와 non-constructor 에는 prototype 프로퍼티가 없음

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // false
```



prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때, 생성자 함수가 생성할 **인스턴스의 프로토타입 객체**를 가리킨다.

<img src="https://github.com/Seongtaek-H/TIL/blob/master/%EB%94%A5%EB%8B%A4%EC%9D%B4%EB%B8%8C%EC%8A%A4%ED%84%B0%EB%94%94/18.2.6.png?raw=true" align="left" width="450px"/>