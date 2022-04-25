# 16장 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

> **내부 슬롯**과 **내부 메서드**는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 `ECMAScript` 사양에서 사용하는 **의사 프로퍼티**와 **의사 메서드**다.

아래의 이중 대괄호로 감싼 이름들이 **내부 슬롯**과 **내부 메서드**다.
![](https://velog.velcdn.com/images/jiseung/post/03b6f8fb-bde4-4c1e-af85-25d6af86b555/image.png)

**내부 슬롯**과 **내부 메서드**는 자바스크립트 엔진에서 실제로 동작하지만 외부로 공개된 객체의 프로퍼티는 아니다.

모든 객체는 `[[Prototype]]`이라는 **내부 슬롯**과 **내부 메서드**를 갖는다. **내부 슬롯**은 자바스크립트 엔진의 내부 로직이므로 원칙적으로 직접 접근할 수 없지만, `[[Prototype]]` 내부 슬롯의 경우, `__proto__`를 통해 간접적 접근할 수 있다.

```js
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__ // Object.prototype
```

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

> 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 **프로퍼티 어트리뷰트**를 기본값으로 자동 정의한다.

- 프로퍼티 어트리뷰트 : 프로터피 상태를 나타내는 것
- 프로퍼티의 상태 : **프로퍼티의 값**, **값의 갱신 가능 여부**, **열거 가능 여부**, **재정의 가능 여부**
- 프로퍼티 디스크립터 객체 : 프로퍼티 어트리뷰트 정보를 제공하는 것

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`이다.
프로퍼티 어트리뷰트는 `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수 있다.

`Object.getOwnPropertyDescriptor` 메서드를 호출할 때 첫 번째 매개변수에는 `객체의 참조`를 전달하고, 두번째 매개변수에는 `프로퍼티 키`를 문자열로 전달한다.

ES8의 `Object.getOwnPropertyDescriptors` 메서드를 호출하면 프로퍼티 디스크립터 객체들을 반환한다.

```js
const person = {
  name: 'Lee'
};

// 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크르립터를 요구하면 undefined를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person);
/*
{
	name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

## 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분된다

- **데이터 프로퍼티** : 키와 값으로 구성된 일반적인 프로퍼티
- **접근자 프로퍼티** : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 **접근자 함수**로 구성된 프로퍼티

### 데이터 프로퍼티

다음과 같은 프로퍼티 어트리뷰트를 갖는다. 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                                                                                                                                                             |
| ------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [[Value]]           | value                               | 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값<br>프로퍼티 키를 통해 프로퍼티 값을 변경하면 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 값을 저장                    |
| [[Writable]]        | writable                            | 프로퍼티 값의 변경 가능 여부<br>false인 경우 읽기 전용                                                                                                                                           |
| [[Enumerable]]      | enumerable                          | 프로퍼티의 열거 가능 여부<br>false인 경우 for...in, Object.keys 등으로 열거할 수 없다.                                                                                                           |
| [[Configurable]]    | configurable                        | 프로퍼티의 재정의 가능 여부<br>false인 경우 프로퍼티 삭제/프로퍼티 어트리뷰트 값 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]] 변경과 [[Writable]]을 false로 변경하는 것이 허용된다. |

```js
const person = {
  name: 'Lee'
};

// 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크르립터를 요구하면 undefined를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person);
/*
{
	name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 **접근자 함수**로 구성된 프로퍼티

다음과 같은 프로퍼티 어프티뷰트를 갖는다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                                                                                  |
| ------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| [[Get]]             | get                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수. getter 함수가 호출되고 결과가 반환된다.   |
| [[Set]]             | set                                 | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수. setter 함수가 호출되고 결과가 저장된다. |
| [[Enumerable]]      | enumerable                          | 데이터 프로퍼티와 같다                                                                                                |
| [[Configurable]]    | configurable                        | 데이터 프로퍼티와 같다                                                                                                |

```js
const person = {
  firstName: "Jiseung",
  lastName: "Kang",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName() {
  	[this.firstName, this.lastName] = name.split(' ');
  }
};

console.log(person.firstName + ' ' + person.lastName); // Jiseung Kang

person.fullName = 'Jiseung Kang';
console.log(person); // firstName: "Jiseung", lastName: "Kang"}

console.log(person.fullName); // Jiseung Kang

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: "jiseung", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}
```

> 접근자 프로퍼티는 자체적으로 값(value)을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장하는데 관여한다.

- **내부 슬롯/메서드 관점**

1. 프로퍼티 키가 유효한지(문자열, 심벌) 확인한다.
2. 프로토타입 체인에서 프로퍼티를 검색한다.
3. 검색된 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
4. 프로퍼티의 프로퍼티 어트리뷰트의 값을 반환한다.

- **프로토타입**

  - 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체
  - 프로토타입은 하위(자식) 객체에 자신의 프로퍼티와 메서드를 상속한다.
  - 상속받은 하위 객체는 상위 객체의 프로퍼티와 메서드를 자유롭게 사용할 수 있다.

- **프로토타입 체인**
  - 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속구조
  - 객체의 프로퍼티나 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티나 메서드가 없다면 프로토타입 체인을 따라 차례대로 검색한다.

### 접근자 프로퍼티와 데이터 프로퍼티를 구별하는 방법

> `Object.getOwnPropertyDescriptor`가 반환한 프로퍼티 디스크립터 객체에서 살펴볼 수 있다.

```js
Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
// {get: f, set: f, enumerable: false, configurable: true}
Object.getOwnPropertyDescriptor(function () {}, "prototype");
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

## 프로퍼티 정의

> 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나,
> 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것

```js
const person = {};

Object.defineProperty(person, "firstName", {
  value: "Jiseung",
  writable: true,
  enumerable: true,
  configurable: true,
});

Object.defineProperty(person, "lastName", {
  value: "Kang",
});

let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log("firstName", descriptor);
// firstName {value: "jiseung", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
descriptor = Object.getOwnPropertyDescriptor(person, "lastName");
console.log("lastName", descriptor);
// lastName {value: "kang", writable: false, enumerable: false, configurable: false}

// Enumerate false의 결과
console.log(Object.keys(person)); // ["firstName"]

// Writable false의 결과
person.lastName = "Kang"; // 무시됨
delete person.lastName; // 무시됨

// Configurable false의 결과
Object.defineProperty(person, "lastName", { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

Object.defineProperty(person, "fullName", {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },

  set() {
    [this.firstName, this.lastName] = name.split(" ");
  },
  enumerable: true,
  configurable: true,
});

descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log("fullName", descriptor);
// fullName {get: f, set: f, enumerable: true, configurable: true}

person.fullName = "Jiseung Kang";
console.log(person); // {firstName: "Jiseung", lastName: "Kang"}
```

`Object.defineProperty` 메서드로 프로퍼티를 정의할 때, 프로퍼티 디스크립터 객체의 프로퍼티 일부를 생략할 수 있다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| ----------------------------------- | ---------------------------- | -------------------- |
| value                               | [[Value]]                    | undefined            |
| get                                 | [[Get]]                      | undefined            |
| set                                 | [[Set]]                      | undefined            |
| writable                            | [[Writable]]                 | false                |
| enumerable                          | [[Enumerable]]               | false                |
| configurable                        | [[Configurable]]             | false                |

### `Object.defineProperty` vs `Object.defineProperties`

```js
const person = {};

Object.defineProperties(person, {
  firstName: {
    value: "Jiseung",
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: "Jiseung",
    writable: true,
    enumerable: true,
    configurable: true,
  },
  fullName: {
    get() {
      return `${this.fullName} ${this.lastName}`;
    },
    set(name) {
      [this.firstName, this.lastName] = name.split(" ");
    },
    enumerable: true,
    configurable: true,
  },
});

person.fullName = "Jiseung Kang";
console.log(person); // {firstName: "Jiseung", lastName: "Kang"}
```

## 객체 변경 방지

> 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다.

| 구분           | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | ------------------------ | ------------- | ------------- | ---------------- | -------------------------- | --- |
| 객체 확장 금지 | Object.preventExtensions | X             | O             | O                | O                          | O   |
| 객체 밀봉      | Object.seal              | X             | X             | O                | O                          | X   |
| 객체 동결      | Object.freeze            | X             | X             | O                | X                          | X   |

### 객체 확장 금지

- `Object.preventExtensions`
  - 객체의 확장 금지 (프로퍼티 추가 금지). 즉, 프로퍼티 동적 추가와 Object.defineProperty 메서드 추가 금지

```js
const person = { name: "Jiseung" };

console.log(Object.isExtensible(person)); // true

Object.preventExtensions(person);

console.log(Object.isExtensible(person)); // false

person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

delete person.name;
console.log(person); // {}

Object.defineProperty(person, "age", { value: 20 });
// TypeError: Cannot define property age, object is not extensible
```

### 객체 밀봉

- `Object.seal`
  - 객체를 밀봉 (프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지). 즉, 읽기와 쓰기만 가능

```js
const person = { name: "Jiseung" };

console.log(Object.isSealed(person)); // false

Object.seal(person);

console.log(Object.isSealed(person)); // true

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
	name: {value: "Jiseung", writable: true, enumerate: true, configurable: false},
}
*/

person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

person.name = "Kang"; // 가능
console.log(person); // {name: "Kang"}

Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

### 객체 동결

- `Object.freeze`
  - 객체를 동결 (프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지). 읽기만 가능

```js
const person = { name: "Jiseung" };

console.log(Object.isFrozen(person)); // false

Object.freeze(person);

console.log(Object.isFrozen(person)); // true

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
	name: {value: "Jiseung", writable: false, enumerate: true, configurable: false},
}
*/

person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

person.name = "Kang"; // 무시. strict mode에서는 에러
console.log(person); // {name: "Jiseung"}

Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

### 불변 객체

> 지금까지 살펴본 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경되고 중첩 객체까지 영향을 주지 못한다. 따라서 동결 객체도 중첩 객체까지 동결할 수는 없다.

```js
const person = {
  name: 'Kang',
  address: { city: 'Seoul' }
},

Object.freeze(person);

console.log(Object.isFrozen(person)); // true

console.log(Object.isFrozen(person.address)); // false

person.address.city = "Jeju";
console.log(person); // {name: "Kang", address: {city: "Jeju"}}
```

> 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```js
function deepFreeze(target) {
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
      Object.freeze(target);
      Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: 'Kang',
  address: { city: 'Seoul' }
},

deepFreeze(person);

console.log(Object.isFrozen(person)); // true

console.log(Object.isFrozen(person.address)); // true

person.address.city = "Jeju";
console.log(person); // {name: "Kang", address: {city: "Seoul"}}
```

# 17장 생성자 함수에 의한 객체 생성

> 생성자 함수를 사용해 객체를 생성하는 방식을 살펴보자. 그리고 객체 리터럴을 사용해 객체를 생성하는 방식과 생성자 함수를 사용해 객체를 생성하는 방식의 장단점을 살펴보자.

## Object 생성자 함수

```js
const person = new Object();

person.name = "Kang";
person.sayHello = function () {
  console.log("Hi! My name is " + this.name);
};

console.log(person); // {name: "Kang", sayHello: f}
person.sayHello(); // Hi! My name is Kang
```

객체를 생성하는 방법을 객체 리터럴을 사용하는 것이 더 간편하다.

> 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수.
> 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.
> 자바스크립트는 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.

## 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

```js
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle2.getDiameter()); // 20
```

객체는 프로퍼티를 통해 객체 고유의 상태를 표현한다.
그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다.

**객체 리터럴에 의한 객체 생성은 직관적이고 간편하지만 단 하나의 객체만 생성한다는 단점이 있다.**

### 생성자 함수에 의한 객체 생성 방식의 장점

> 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다

```js
function Circle(radius) {
  this.radius: 5,
  this.getDiameter() {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- this

  - this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이다.
  - this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

    | 함수 호출 방식       | this가 가리키는 값(this 바인딩)        |
    | -------------------- | -------------------------------------- |
    | 일반 함수로서 호출   | 전역 객체                              |
    | 메서드로서 호출      | 메서드를 호출한 객체(마침표 앞의 객체) |
    | 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스          |

  ```js
  function foo() {
    console.log(this);
  }

  foo(); // window

  const obj = { foo };
  obj.foo(); // obj;

  const inst = new foo(); // inst
  ```

- 생성자 함수
  - 객체(인스턴스)를 생성하는 함수
  - new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

```js
const circle3 = Circle(15);
console.log(circle3); // undefined
console.log(radius); // 15
```

### 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할 : 인스턴스를 생성하는 것. 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것

> 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.
> 그 과정을 살펴보자

```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 여기서 명시적으로 다른 객체를 반환하면 암묵적인 this 반환이 무시된다.
  // 명시적으로 원시 값을 반환하면 무시하고 this가 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle1 = new Circle(5);
console.log(circle); // Circle { radius: 1, gitDiameter: f}
```

1. (런타임 이전) 인스턴스 생성과 this 바인딩
   암묵적으로 빈 객체(인스턴스)가 생성되고 `this`에 바인딩된다.

- 바인딩 : 식별자와 값을 연결하는 과정

2. 인스턴스 초기화
   코드가 한 줄씩 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
   `this`에 바인딩된 인스턴스에 프로퍼티나 메서드를 추가하고
   생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당해 초기화하거나 고정값을 할당한다.

3. 인스턴스 반환
   생성자 함수 내부 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.
   만약 this가 아닌 다른 객체를 명시적으로 반환하면 return문에 명시한 객체가 반환된다.
   하지만 명시적으로 원시 값을 반환하면 무시하고 this가 반환된다.
   => 이러한 이유로 생성자 함수 내부 return 문을 반드시 생략해야 한다.

### 내부 메서드 [[Call]]과 [[Construct]]

> 함수 선언문, 함수 표현식으로 정의한 함수는 일반적인 함수 뿐만 아니라 생성자 함수로서 new 연산자와 함께 호출해 객체를 생성할 수 있다.

함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다.
(함수 객체는 일반 객체처럼 내부 슬롯과 내부 메서드를 모두 가지고 있다.)

```js
function foo() {}

foo.prop = 10;

foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```

> 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
> 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고,
> 함수가 new 연산자와 함께 생성자 함수로 호출되면 내부 메서드 [[Construct]]가 호출된다.

```js
function foo() {}
foo(); // [[Call]]
new foo(); // [[Construct]]
```

함수 객체를 `callable`이라 하며, 생성자 함수로 호출할 수 있으면 `constructor`, 아니면 `non-constructor`라고 부른다.

### constructor와 non-constructor의 구분

`constructor` : 함수 선언문, 함수 표현식, 클래스
`non-constructor` : 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
function foo() {}
const bar = function () {};
const baz = {
  x: function () {}
};

new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

const obj - {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현 만을 의미한다.
즉, 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다.

```js
function foo() {}

foo(); // [[Call]]이 호출된다.

new foo(); // [[Construct]]가 호출된다. 없다면 에러가 발생한다.
```

일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다.

### new 연산자

```js
function add(x, y) {
  return x + y;
}

let inst = new add();

console.log(inst); // {}

function createUser(name, role) {
  return { name, role };
}

inst = new createUser("Kang", "admin");
console.log(inst); // {name: "Kang", role: "admin"}

function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
const circle = Circle(5);
console.log(circle); // undefined
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부 `this`는 `Circle` 생성자 함수가 생성할 인스턴스를 가리킨다.
하지만 일반 함수로 호출하면 함수 내부 `this`는 전역 객체 `window`를 가리킨다.

> 생성자 함수는 파스칼 케이스로 명명해 일반 함수와 구별할 수 있도록 하자

### new.target

> 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 new.target을 지원한다.

new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. 일반 함수로 호출되면 undefined다.

```js
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

#### 스코프 세이프 생성자 패턴

> new.target은 ES6에서 도입된 패턴으로 IE에서 지원하지 않는다. 이때는 이러한 패턴을 사용할 수 있다.

```js
function Circle(radius) {
  if(!this instanceof Circle)) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

대부분의 빌트인 생성자 함수들은 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환하므로
new 연산자 없이 호출해도 함께 호출했을 때와 동일하게 동작한다.

하지만 `String`, `Number`, `Boolean` 생성자 함수는 new 연산자와 호출하면 `String`, `Number`, `Boolean` **객체**를 생성해 반환하지만

new 연산자 없이 호출하면 `문자열`, `숫자`, `불리언` 값을 변환한다.
(이를 통해 데이터 타입을 변환하기도 한다.)
