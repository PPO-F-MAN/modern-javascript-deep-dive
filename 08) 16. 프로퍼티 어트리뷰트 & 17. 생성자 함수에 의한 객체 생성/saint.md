# 16. 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티(psedo property)와 의사 메서드(psedo method)

ECMAScript 사양에서 등장하는 이중 대괄호 ([[...]]) 감싼 이름들이 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 ECMAScript 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근하거나 호출할 수 없음

예를 들어, 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지나 직접 접근하지 못하고 `__proto__` 를 통해 간접적으로 접근할 수 있음

```js
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // Uncaught SyntaxError : Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단 제공
O.__proto__ // -> Object.prototype
```



## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 **`프로퍼티 어트리뷰트`**를 기본값으로 자동 정의함

프로퍼티 상태 : 프로퍼티의 값(value), 값의 갱신 가능 여부(writeable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable) 

프로퍼티 어트리뷰트는 내부 슬롯이라 직접 접근할 수 없지만, Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인 가능

Object.getOwnPropertyDescriptor 메서드는 하나의 프로퍼티에 대해 프로퍼티 어트리뷰트 정보를 제공하는 **`프로퍼티 디스크립터`**(PropertyDescriptor) 객체를 반환

ES8에서 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환

```js
const person = {
  name: 'Lee'
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체 반환
// 첫 번째 매개변수에는 객체의 참조를 전달, 두 번째 매개변수에는 프로퍼티 키를 문자열로 전달
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value : "Lee", writable: true, enumerable: true, configurable: true}

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환
console.log(Object.getOwnPropertyDescriptors(person));
/* 
{
	name: {value : "Lee", writable: true, enumerable: true, configurable: true},
	age: // {value : 20, writable: true, enumerable: true, configurable: true}
} 
*/
```



## 16.3 데이터 프로퍼티와 접근자 프로퍼티

데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티

접근자 프로퍼티 : 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

데이터 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖고, 이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의됨

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 내용                                                         |
| :-----------------: | :---------------------------------: | ------------------------------------------------------------ |
|      [[Value]]      |                value                | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값<br />- 프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당. <br />  이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값 저장 |
|    [[Writable]]     |              writable               | - 프로퍼티 값의 변경 가능 여부. 불리언 값을 가짐<br />- [[Writable]] 값이 false면 해당 프로퍼티 [[Value]] 값을 변경 못하는 읽기 전용 프로퍼티가 됨 |
|   [[Enumerable]]    |             enumerable              | - 프로퍼티의 열거 가능 여부. 불리언 값을 가짐<br />- [[Enumerable]] 값이 false면 해당 프로퍼티는 for...in문이나 Object.keys 등으로 열거 불가 |
|  [[Configurable]]   |            configurable             | - 프로퍼티 재정의 가능 여부. 불리언 값을 가짐<br />- [[Configurable]] 값이 false면 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경 금지<br />  단, [[Writable]]이 true인 경우 [[Value]] 변경과 [[Writable]]을 false로 변경하는 건 가능 |

프로퍼티가 생성될 때, [[Value]]의 값은 프로퍼티 값으로 초기화되고, [[Writable]], [[Enumerable]], [[Configurable]] 값은 true 로 초기화



### 16.3.2 접근자 프로퍼티

접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 내용                                                         |
| :-----------------: | :---------------------------------: | ------------------------------------------------------------ |
|       [[Get]]       |                 get                 | 접근자 프로퍼티를 통해 데이트 프로퍼티의 값을 읽을 때 호출되는 접근자 함수<br />접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값,<br />즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환됨 |
|       [[Set]]       |                 set                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수<br />접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값,<br />즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장됨 |
|   [[Enumerable]]    |             enumerable              | 데이터 프로퍼티의 [[Enumerable]] 와 같음                     |
|  [[Configurable]]   |            configurable             | 데이터 프로퍼티의 [[Configurable]] 와 같음                   |

접근자 함수는 getter/setter 함수라고도 부르며, 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고, 하나만 정의할 수도 있다.

접근자 프로퍼티는 자체적으로 값(프로퍼티 어트리뷰트 [[Value]])을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여

```js
const person = {
  // 데이터 프로퍼티
  firstName: 'Ungmo',
  lastName: 'Lee',
  
  // fullName은 접근자 함수로 구성된 접근자 프로퍼티
  // getter 함수
  get fullName(){
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name){
    // 배열 디스트럭처링 할당 (31.1 배열 디스트럭처링 할당 참고)
    [this.fistNmae, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastNmae); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출됨
person.fullName = 'Heegun Lee';
console.log(person); // {fistName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출됨
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티
// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
consoel.log(descriptor);
// {value : "Heegun", writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티
// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}
```



## 16.4 프로퍼티 정의

프로퍼티 정의 : 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것

`Object.defineProperty` 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.

`object.defineProperties` 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

`Object.defineProperty(obj, property, descriptor)`

```js
const person = {}

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
  value: 'Lee'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('fistName', descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}
// [[Writeable]] 의 값이 false인 경우 해당 프로퍼티의 [[Value]] 값을 변경할 수 없다.
// [[Enumerable]] 의 값이 false인 경우 해당 프로퍼티는 for...in 이나 object.keys 등으로 열거할 수 없다.
// [[Configurable]] 의 값이 false인 경우 해당 프로퍼티를 삭제 및 재정의 할 수 없다.

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get(){
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name){
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true
});

descriptor = object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName {get: f, set: f, enumerable: true, configurable: ture}

person.fullName = "Heegun Lee";
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```



## 16.5 객체 변경 방지

객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있는데, 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

|      구분      | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| :------------: | :----------------------- | :-----------: | :-----------: | :--------------: | :--------------: | :------------------------: |
| 객체 확장 금지 | object.preventExtensions |       X       |       O       |        O         |        O         |             O              |
|   객체 밀봉    | object.seal              |       X       |       X       |        O         |        O         |             X              |
|   객체 동결    | object.freeze            |       X       |       X       |        O         |        X         |             X              |

### 16.5.1 객체 확장 금지

`object.preventExtensions` 메서드는 객체의 확장을 금지한다. 객체의 확장 금지란 **프로퍼티 추가 금지**를 의미한다.

### 16.5.2 객체 밀봉

`object.seal` 메서드는 객체를 밀봉한다. 객체의 밀봉이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미한다. 즉, 밀봉된 객체는 **읽기와 쓰기만 가능**하다.

### 16.5.3 객체 동결

`object.freeze` 메서드는 객체를 동결한다. 객체 동결이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미한다.

즉, 동결된 객체는 **읽기만 가능**하다.

### 16.5.4 불변 객체

object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수는 없다. 

객체의 중첩 객체까지 동결하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 object.freeze 메서드를 호출해야 한다.



# 17. 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

new 연산자와 함께 object 생성자 함수를 호출하면 빈 객체를 생성하여 반환

빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function(){
  console.log('Hi! My name is' + this.name);
}

console.log(person); // {name: "Lee", sayHello: f}
person.sayHello(); // Hi! My name is Lee
```

**`생성자 함수 (Constructor)`** : new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수

**`인스턴스`** : 생성자 함수에 의해 생성된 객체

자바스크립트는 Object 생성자 함수 외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공



## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하지만, 단 하나의 객체만 생성하기 때문에 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야하는 경우 비효율적이다.

프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다.

```js
const circle1= {
  radius: 5,
  getDiameter(){
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2= {
  radius: 10,
  getDiameter(){
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
```



### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

자바스크립트에서는 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수가 생성자 함수로 동작한다.

만약 new 연산자와 함께 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```js
// 생성자 함수
function Circle(radius){
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function(){
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```



### 17.2.3 생성자 함수의 인스턴스 생성 과정

new 연산자와 함께 생성자 함수를 호출하면 인스턴스를 생성하고, 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)한 후 암묵적으로 인스턴스를 반환한다.

#### 	1. 인스턴스 생성과 this 바인딩

​		암묵적으로 빈 객체(인스턴스)를 생성하고, 이는 this에 바인딩된다. 이 처리는 런타임 이전에 실행된다.

#### 	2. 인스턴스 초기화

​		생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다. (프로퍼티, 메서드 추가 및 인수로 전달받은 초기값 할당)

#### 	3. 인스턴스 반환

​		생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

``` js
function Circle(radius){
  // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
  
  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function(){
    return 2 * this.radius;
  };
  
  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

만약 this가 아닌 다른 객체를 명시적으로 반환하면 return 문에 명시한 객체가 반환되므로 생성자 함수 내부에서 return 문은 반드시 생략한다.



### 17.2.4 내부 메서드 [[Call]] 과 [[Construct]]

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Constuct]]가 호출된다.

```js
function foo(){}

// 일반적인 함수로서 호출: [[Call]] 호출
foo();

// 생성자 함수로서 호출: [[Construct]] 호출
new foo();
```

callable : 내부 메서드 [[Call]] 을 갖는 함수 객체

constructor : 내부 메서드 [[Construct]] 를 갖는 함수 객체

non-constructor : [[Construct]] 를 갖지 않는 함수 객체

함수 객체는 callable이면서 constructor이거나, callable이면서 non-constuructor.  

모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.

<img src="https://velog.velcdn.com/images%2Fminj9_6%2Fpost%2F91de10a7-f2bc-40f6-ac11-065551461f46%2Fimage.png" align="left"/>



### 17.2.5 constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분

* constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
* non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수



### 17.2.6 new 연산자

일반 함수와 생성자 함수에 특별한 형식 차이는 없으며, new 연산자와 함께 함수를 호출하면 해당 함수는 생성자로 동작. 단, constructor이어야 한다.

반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.

생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력한다.



### 17.2.7 new.target

생성자 함수가 new 연산자 없이 호출되는 위험을 방지하기 위해 ES6에서는 new.target을 지원한다.(IE 지원 X)

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.

new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined이다.

```js
// 생성자 함수
function Circle(radius){
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined
  if(!new.target){
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function(){
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

ES6 이전이나 IE에서는 스코프 세이프 생성자 패턴을 사용한다.

```js
  if(!(this instanceof Circle)){
    // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }
```
