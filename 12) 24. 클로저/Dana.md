# 클로저
자바스크립트의 고유 개념이 아닌, 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성
> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합

#### 함수가 선언된 렉시컬 환경
```js
const x = 1;

function outerFunc(){
  const x = 10;
  
  function innerFunc() {
    console.log(x); // 10
  }
  
  innerFunc();
  // innerFunc이 outerFunc 함수 안에서 **정의**되었기 때문에, innerFunc의 상위스코프는 outerFunc
  // 만약 innerFunc가 정의된 것이 아니라 단순히 호출되었다면, innerFunc은 outerFunc의 변수 x에 접근할 수 없음.
}

outerFunc();
```

```js
const x = 1;

function outerFunc(){
  const x = 10;

  innerFunc();
}

function innerFunc() {
  console.log(x); // 1
}
  
outerFunc();
```

## 24.1 렉시컬 스코프
자바스크립트 엔진이 함수를 어디서 **호출**했는지가 아니라, 함수를 어디서 **정의**했는지에 따라 상위스코프를 결정하는 것.
이것에 의해 상위스코프는 정적으로 결정되고, 변하지 않음. 

> `스코프` = 실행컨텍스트의 렉시컬 환경
`스코프 체인` = 외부 렉시컬 환경에 대한 참조를 통해 상위 렉시컬 환경과 연결됨

## 24.2 함수 객체의 내부 슬롯 `[[Environment]]`

상위 스코프를 기억하기 위해, 함수는 자신의 내부 슬롯 `[[Environment]]`에 참조를 저장한다.

함수 정의가 평가되어 함수 객체를 생성하는 시점
= 상위스코프가 평가 또는 실행되고 있는 시점
= 실행 중인 실행 컨텍스트 == 상위 스코프의 실행 컨텍스트

이 시점에 `[[Environment]]` 에 상위 스코프 참조가 할당됨.

## 24.3 클로저와 렉시컬 환경
```js
const x = 1;

function outer() {
  const x = 10;
  const inner = function () { console.log(x) };
  return inner;
}

const innerFunc = outer();
// outer 함수를 호출하면 outer함수는 **중첩함수 inner를 반환하고** 생명 주기를 마감
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거 됨.
innerFunc();
```

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조 가능 -> 이 때 중첩 함수를 클로저라고 함.
실행 컨택스트에서 제거되어도 렉시컬 환경까지 소멸하는 것은 아님

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저이지만, 상위스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아님
이런 경우에는 브라우저 최적화를 통해 상위스코프를 기억하지 않음. 

중첩함수가 외부함수보다 먼저 소멸되는 경우도 클로저라고 하지 않는다.

> 만약 중첩 함수가 외부 함수의 모든 식별자를 사용하지 않는다면, 상위 스코프에는 사용하는 식별자만 저장됨. 이 때 사용되는 식별자를 `자유 변수`라고 부름.

## 24.4 클로저의 활용
상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용

### 예시 1)
```js
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function() {
  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

1. 카운트 상태(num 변수의 값)는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 카운트 상태(num변수의 값)는 increase 함수만이 변경할 수 있어야 한다.

여기선 전역변수로 변수가 정의되어, 암묵적 결합이 일어남.
> #### 암묵적 결합
전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있는 상태

따라서 지역변수로 값을 설정해 외부에서는 접근하지 못하도록 해야함.

### 예시 2)
```js
const increase = function(){
  let num = 0;
  
  return ++num;
}

// 이전 상태를 유지 하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
```

### 예시 3)
```js
const increase = (function () {
  let num = 0;
  
  //클로저
  return function() {
    return ++num;
  };
}());

console.log(increase()); // 1
console.log(increase()); // 2
```

```js
increase = function() {
    return ++num;
  }; // -> 상위 컨텍스트에 num에 대한 정보를 가지고 있음
```

즉시 실행 함수가 반환한 클로저는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억
-> 즉시 실행 함수가 반환한 클로저는 카운트 상태를 유지하기 위한 자유 변수 num을 언제 어디서 호출하든지 참조하고 변경 가능

즉시 실행 함수는 한 번만 실행되므로 초기화될 일이 없고, 외부에서 접근 할 수 없으므로 안정적인 프로그래밍이 가능하다.

다른 기능도 추가하기 위해 다음과 같이 수정할 수 있음.
```js
const counter = (function() {
  // 카운트 상태 변수
  let num = 0;
  
  // 클로저인 메서드를 갖는 객체를 반환
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경
  return {
    increase() {
      return ++num;
    },
   	decrease() {
      return num > 0 ? --num : 0;
    }
  };
}());

console.log(counter.increase());
console.log(counter.decrease());
```

변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생 가능
외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저 사용

```js
function makeCounter(aux){
  let counter = 0;
  
  return function () {
    counter = aux(counter);
    return counter;
  };
}

// 보조함수
function increase(n) {
  return ++n;
}

// 보조함수
function decrease(n) {
	return --n;
}

const increaser = makeCounter(increase)
console.log(increaser()); // 1
console.log(increaser()); // 2
// 같은 increaser 함수끼리 렉시컬환경을 공유함

const decreaser = makeCounter(decrease)
console.log(decreaser()); // -1
// 하지만 makeCounter가 새로 선언되었기 때문에 다른 렉시컬 환경을 공유함.
```

> makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다.

```js
const counter = (function () {
  let counter = 0;
  
  return function (aux){
    counter = aux(counter);
    
    return counter;
  };
}());

// 보조함수
function increase(n) {
  return ++n;
}

// 보조함수
function decrease(n) {
	return --n;
}

console.log(counter(increase));
```
-> 자유 변수를 공유하게 됨.

### 모듈 패턴
네임스페이스 패턴에 렉시컬 스코프를 추가한 패턴
객체에 유효범위를 주어 캡슐화할 때 사용

가장 쉬운 방법은 객체 리터럴을 사용하는 것
```js
function candy() {
  const name = 'kim';
  const count = 10;
  
  return {
    addCandies : add => count + add,
    removeCandies : remove => count - remove,
    showCandies : () => {console.log(count)},
  }
}

candy().addCandies(20);
candy().showCandies()
```


## 24.5 캡슐화와 정보 은닉
`캡슐화` : 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것
`정보 은닉` : 객체의 특정 프로퍼티나 메서드를 감추는 것으로 의도치않게 상태가 변경되는 것을 막고, 객체 간 상호 의존성을 낮춤

```js
const Person = (function () {
  let _age = 0;
  
  function Person(name , age){
    this.name = name;
    _age = age;
  }
  
  Person.prototype.sayHi = function() {
    console.log(_age);
  }
  
  // 생성자 함수 반환
  return Person;
}());

const me = new Person('kim', 20);
me.sayHi() // 20
console.log(me._age) // undefined
```

sayHi는 종료된 즉시실행함수의 지역변수 _age를 참조할 수 있는 클로저
sayHi를 통해서만 age에 접근할 수 있다.

하지만 Person 객체간 _age의 값이 공유되지 않는 단점이 존재함.
-> 자바스크립트는 정보 은닉을 완전하게 지원하지 않음

## 24.6 자주 발생하는 실수
```js
var funcs =[];

for (var i = 0 ; i < 3 ; i++){
  funcs[i] = function() {return i;}
}

for (var j = 0; j<funcs.length; j++){
  console.log(funcs[j]())
}
```
다음 코드에서 0,1,2가 차례대로 출력될 것 같지만, 실제로는 i가 전역변수로 선언되었기 때문에 i의 값이 바뀌는 대로 다같이 바뀌어 3,3,3이 출력됨.

이를 해결하기 위해
1. let, const 등의 es6문법 사용하기
2. 고차 함수 사용하기

## 단점
클로저가 활용된 패턴에서는 가비지 컬렉터의 대상이 되지 않고 메모리상에 존재
따라서 과도하게 남발하면 메모리 누수 현상이 발생할 수 있음
-> 내부 함수의 사용이 더이상 필요없는 경우, 외부변수를 초기화하여 메모리 할당을 해체시켜주는 것이 좋음.

> 다른 더 자세한 내용은 [코어자바스크립트 : 클로저](https://velog.io/@deli-ght/%ED%81%B4%EB%A1%9C%EC%A0%80-%EC%BD%94%EC%96%B4%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-5#%ED%81%B4%EB%A1%9C%EC%A0%80%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%95%B4%EC%A0%9C-%EC%98%88%EC%A0%9C) 참고하기!