# 25. 클래스

## 1. 클래스는 프로토타입의 문법적 설탕인가?

> 자바스크립트는 **프로토타입 기반 객체지향 언어**다.

프로토타입 기반 객체지향 언어는 클래스가 필요없다.
다음과 같이 객체지향 언어의 상속을 구현할 수 있다.

```js
var Person = (function () {
  function Person(name) {
    this.name = name;
  }
  Person.prototype.sayHi = function () {
    console.log(this.name + ": Meow");
  };

  return Person;
})();

var me = new Person("Choon");
me.sayHi(); // Choon : Meow
```

사실 클래스는 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 **문법적 설탕**이라고 볼 수도 있다.

```js
class Person2 {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log(this.name + ": MeowMeow");
  }
}

var you = new Person2("Sik");
you.sayHi(); // Sik : Meow
```

단, 클래스는 생성자 함수보다 엄격하며 몇 가지 다른 기능도 제공한다.

1. 클래스를 **new 연산자 없이 호출하면 에러 발생**한다. (생성자 함수는 new 연산자가 없으면 일반 함수로서 호출된다.)

2. 클래스는 **상속을 지원하는 extends와 super 키워드**를 제공한다.

3. 클래스는 **호이스팅이 발생하지 않는 것처럼 동작**한다. (함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.)

4. 클래스 내의 모든 코드는 암묵적으로 **strict mode**가 지정되어 있고 해제할 수 없다.

5. 클래스는 **constructor, 프로토타입 메서드, 정적 메서드 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false**다. (열거되지 않는다)

> 그러므로 클래스를 단순한 문법적 설탕이라고 보기보다 **새로운 객체 생성 메커니즘**으로 보는 것이 타당하다.

## 2. 클래스 정의

> 클래스는 **class 키워드**를 사용해 정의하며, 생성자 함수처럼 **파스칼 케이스**를 사용하는 것이 일반적이다.

```js
class Person {}

// 이런 것도 가능하다.
const Person = class {}; // 익명
const Person = class MyClass {}; // 기명
```

위처럼 표현식 정의가 가능한 이유는 클래스가 **값**으로 사용할 수 있는 **일급 객체**이기 때문이다.

- 클래스는 **함수**다.
- 클래스는 **값**처럼 사용할 수 있는 **일급 객체**다.

클래스는 일급 객체로서 다음과 같은 특징을 갖는다.

1. 무명의 리터럴로 생성할 수 있다. 런타임에 생성 가능하다.
2. 변수나 자료구조에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

## 3. 클래스 호이스팅

> **클래스는 함수로 평가된다.**

**클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.**

이때 클래스가 평가되어 생성한 함수 객체는 constructor다.

**프로토타입**과 **생성자 함수(constructor)**는 단독으로 존재할 수 없기 때문에, 함수 정의가 평가되어 함수 객체를 생성할 때 프로토타입도 생성된다.

```js
console.log(typeof Person);
class Person {}
```

단, 클래스는 클래스 정의 이전에 참조할 수 없다.

## 4. 인스턴스 생성

> **클래스**는 **생성자 함수**이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.

함수는 new 사용 여부에 따라 일반 함수나 생성자 함수로 호출되지만 클래스는 생성자 함수이므로 반드시 new 연산자와 함께 호출해야 한다.

클래스 표현식으로 정의된 클래스의 경우 다음처럼 기명 클래스 표현식의 클래스 이름으로 인스턴스를 생성할 수 없다.

```js
const Person = class MyClass {};

console.log(MyClass); // ReferenceError: MyClass is not defined
const you = new MyClass(); // ReferenceError: MyClass is not defined
```

## 5. 메서드

클래스 몸체에서 정의할 수 있는 메서드는 **constructor, 프로토타입 메서드, 정적 메서드** 세 가지이다.

```js
class Person {
  // constructor
  constructor(name) {
    this.name = name;
  }
  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! I'm ${this.name}`);
  }
  // 정적 메서드
  static sayHello() {
    console.log("Hello!");
  }
}

const me = new Person("Choon");

console.log(me.name); // Choon
me.sayHi(); // Hi! I'm Choon
Person.sayHello(); // Hello!
```

> 곧 constructor 내부가 아닌 몸체에서도 프로퍼티를 정의할 수 있게될 것 같다. (지금도 된다)

```js
class Person {
  name;
}
```

### constructor

인스턴스를 생성하고 초기화하기 위한 특수한 메서드

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

> 클래스는 인스턴스를 생성하기 위한 생성자 **함수**다.

클래스는 평가되어 **함수 객체**가 된다.

클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있고, 프로토타입과 연결되어 있으며, 자신의 스코프 체인을 구성한다.

모든 함수 객체가 가진 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킨다.

constructor 내부의 this는 생성자 함수처럼 클래스가 생성한 인스턴스를 가리킨다.

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

function Person(name) {
  this.name = name;
}
```

> constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.
> 클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티는 이름이 같지만 다르다.

**클래스의 constructor**는..

- 클래스 내에 0~1개만 존재한다.
- 생략시 빈 constructor가 암묵적으로 정의된다.
- 인스턴스를 초기화하려면 constructor를 생략하면 안된다.
- 별도의 반환문을 갖지 않는다. (반드시 생략하자)
- 반환문이 있으면 this 반환이 무시되어 인스턴스가 반환되지 못한다. (원시값 반환시에는 원시값 반환을 무시한다.)

### 프로토타입 메서드

생성자 함수를 사용해 인스턴스를 생성하는 경우, 프로토타입 메서드를 생성하기 위해 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야 한다.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () {
  console.log(`Hi ${this.name}`);
};

const me = new Person("Choon");
me.sayHi(); // Hi Choon
```

클래스 몸체의 메서드는 기본적으로 프로토타입 메서드가 된다!

```js
function Person(name) {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}

const me = new Person('Choon');
me.sayHi(); // Hi Choon

console.log(Object.getPrototypeOf(me) === Person.prototype); // true
console.log(me instanceof Person); // true

console.log(Object.getPrototypeOf(Person.prototype) === Object.prototype); // true
console.log(me instanceof Object); // true

console.log(me.constructor === Person) // true
```

클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 되며, 인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다.

### 정적 메서드

> 정적 메서드 : 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

생성자 함수의 경우 명시적으로 생성자 함수에 메서드를 추가해야 하지만 클래스에서는 메서드에 static 키워드를 붙이면 된다.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  static sayHi() {
    console.log("Hi!");
  }
}

Person.sayHi();

const me = new Person("Choon");
me.sayHi(); // TypeError: me.sayHi is not a function
```

클래스는 함수 객체로 평가된다.
그러므로 자신의 프로퍼티/메서드를 소유할 수 있다.

정적 메서드가 바인딩 된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않는다.

### 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 **클래스**로 호출하고 프로토타입 메서드는 **인스턴스**로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```js
class Square {
  static area(width, height) {
    return width * height;
  }
}

console.log(Square.area(10, 10));

class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  area(width, height) {
    return width * height;
  }
}

const square = new Square(10, 10);
console.log(square.area());
```

프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다르다.

메서드 내부에서 인스턴스 프로퍼티를 참조하려면 this를 사용해야 하며, 프로토타입 메서드로 정의해야 한다.

**this를 사용하지 않는 메서드는 정적 메서드로 정의하자**

표준 빌트인 객체 Math, Number, JSON, Object, Reflect 등은 다양한 정적 메서드를 가진다.

```js
Math.max(1, 2, 3);
Number.isNaN(NaN);
JSON.stringify({ a: 1 });
Object.id({}, {});
Reflect.has({ a: 1 }, "a");
```

이런 식으로 클래스나 생성자 함수를 하나의 **네임스페이스**로 사용해 정적 메서드를 모아놓으면 이름 충돌 가능성을 줄여주고 관련 함수를 구조화할 수 있다.

### 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 **메서드 축약 표현**을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때 **콤마**가 필요 없다.
3. 암묵적으로 **strict mode**로 실행된다.
4. for ... in 문이나 Object.keys 메서드 등으로 **열거할 수 없다.**
5. 내부 메서드 [[Contruct]]를 갖지 않는 **non-constructor** new 연산자와 호출할 수 없다.

## 6. 클래스의 인스턴스 생성 과정

new 연산자와 함께 클래스를 호출하면 클래스의 내부 메서드 [[Contruct]]가 호출된다.

```js
class Person {
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

## 7. 프로퍼티

### 인스턴스 프로퍼티

constructor 내부에서 정의해야 한다.

```js
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name;
  }
}

const me = new Person("Choon");
console.log(me); // Person { name: "Choon"}
```

### 접근자 프로퍼티

접근자 프로퍼티는 접근자 함수로 구성된 프로퍼티다.

```js
const person = {
  firstName: 'Sik';
  lastName: 'Choon';

  get fullName() {
    return `${lastName} ${firstName}`;
  },
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

console.log(`${person.lastName} ${person.firstName}`); // Choon Sik

person.fullName = 'Choon Tai';
console.log(person); // { firstName: 'Tai', lastName: 'Choon'}

console.log(person.fullName) // Choon Tai

console.log(Obejct.getOwnPropertyDescriptor(person, 'fullName')); // {get: f, set: f, enumerable: true, configurable: true}
```

클래스

```js
class Person = {
  constructor(firstName, lastName) {
    this.firstName: firstName;
    this.lastName: lastName;
  }

  get fullName() {
    return `${lastName} ${firstName}`;
  },
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

const me = new Person('Sik', 'Choon');

console.log(`${me.lastName} ${me.firstName}`); // Choon Sik

me.fullName = 'Choon Tai';
console.log(me); // { firstName: 'Tai', lastName: 'Choon'}

console.log(me.fullName) // Choon Tai

console.log(Obejct.getOwnPropertyDescriptor(Person.prototype, 'fullName')); // {get: f, set: f, enumerable: false, configurable: true}

Object.getOwnPropertyNames(me); // ["firstName", "lastName"]
Object.getOwnPropertyNames(Object.getPrototypeOf(me)); // ["constructor", "fullName"]
```

### 클래스 필드 정의 제안

클래스 필드 : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티

자바

```java
public class Person {
  // 클래스 필드
  private String firstName = "";
  private String lastName = "";
  // 생성자
  Person(String firstName, String lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  // 클래스 필드 참조
  public String getFullName() {
    return firstNAme + " " + lastName;
  }
}
```

자바스크립트

```js
class Person {
  name = "Lee";
  // name; // 초기화하지 않으면 undefined

  constructor() {
    console.log(name); // ReferenceError: name is not defined
  }
}
const me = new Person();
console.log(me); // Person { name: "Lee" }

class Person {
  this.name = "Lee"; // SyntaxError: Unexpected tocken '.'
}
```

```js
class Person {
  // name;

  constructor(name) {
    this.name = name;
  }
}
const me = new Person("Choon");
console.log(me); // Person { name: "Choon" }
```

```js
class Person {
  name = "Choon";

  getName = function () {
    return this.name;
  };
  // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this
  // getName = () => this.name;
}
const me = new Person();
console.log(me); // Person { name: "Choon" }
console.log(me.getName()); // Choon
```

### private 필드 정의 제안

> private 필드의 선두에는 #을 붙여준다. private 필드를 참조할 때도 #을 붙여주어야 한다.

```js
class Person {
  #name = "";
  constructor(name) {
    this.#name = name;
  }
}

const me = new Person("Choon");
console.log(me.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
```

> 타입스크립트는 public, private, protected를 모두 지원한다.

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | O       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

> 다만 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.

```js
class Person {
  #name = ""; // 반드시 클래스 몸체에 정의해야 한다.
  constructor(name) {
    this.#name = name;

    get name() {
      return this.#name.trim();
    }
  }
}

const me = new Person(' Choon ');
console.log(me.name); // Choon
```

### static 필드 정의 제안

> Static class features : static public/private 필드, static private 메서드

```js
class MyMath {
  // static public
  static PI = 22 / 7;
  // static private
  static #num = 10;
  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

## 8. 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

> 상속에 의한 클래스 확장 : 기존 클래스를 상속받아 새로운 클래스를 확장(extends)하여 정의하는 것

```js
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() {
    return "eat";
  }
  move() {
    return "move";
  }
}

class Bird extends Animal {
  fly() {
    return "fly";
  }
}

const bird = new Bird(1, 5);
console.log(bird); // Bird {age: 1, weigth: 5}
console.log(bird instanceOf Bird); // true
console.log(bird instanceOf Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

### extends 키워드

```js
// super-class, base class, parent class
class Base {}
// subclass, derived class, child class
class Derived extends Base {}
```

이들은 클래스 간의 프로토타입 체인도 생성하며, 이를 통해 프로토타입 메서드와 정적 메서드 모두 상속이 가능하다.

### 동적 상속

extends 키워드는 생성자 함수 등 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 상속받을 수도 있지만,

상속 받는 대상은 무조건 클래스다.

```js
function Base(a) {
  this.a = a;
}

class Derived extends Base {}
const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

```js
function Base1() {}

class Base2 {}

let condition = true;

class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceOf Base1); // true
console.log(derived instanceOf Base2) // false
```

### 서브클래스의 constructor

```js
class Base {
  // constructor() {}
}

class Derived extends Base {
  // super()는 super-class의 constructor를 호출해 인스턴스를 생성한다.
  // constructor(...args) { super(...args); }
}

const derived = new Derived();
console.log(derived); // Derived {}
```

### super 키워드

> super 키워드는 함수처럼 호출하거나 this처럼 참조할 수 있다.

1. super를 호출하면 super-class의 constructor를 호출한다.

```js
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class Derived extends Base {}

class Derived2 extends Base {
  constructor(a, b, c) {
    super(a, b); // constructor를 생략하지 않는 경우 super를 반드시 호출해야 한다.
  }
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}

const derived2 = new Derived(1, 2, 3);
console.log(derived2); // Derived2 {a: 1, b: 2, c: 3}
```

- 서브클래스의 constructor를 생략하지 않는 경우 super를 반드시 호출해야 한다.

- 서브클래스의 constructor에서 super를 호출하기 전에 this를 참조할 수 없다.

- super는 반드시 서브클래스의 constructor에서만 호출할 수 있다.

2. super를 참조하면 super-class의 메서드를 호출할 수 있다.

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi ${this.name}`;
  }
}

class Derived extends Base {
  sayHi() {
    // const __super = Object.getPrototypeOf(Derived.prototype);
    // return `${__super.sayHi.call(this)}. how are you doing?`;
    return `${super.sayHi()}. how are you doing?`;
  }
}

const derived = new Derived("Choon");
console.log(derived.sayHi()); // Hi Choon. how are you doing?
```

super는 자신을 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 가리킨다.

서브클래스의 프로토타입 메서드 내에서 super.sayHi는 super-class의 프로토타입 메서드 sayHi(Base.prototype.sayHi)를 가리킨다.

단, Base.prototype.sayHi를 호출할 때 call 메서드를 사용해 this를 전달해야한다.
Base.prototype.sayHi를 그대로 호출하면 Base.prototype.sayHi 메서드 내부의 this는 Base.prototype을 가리키는데, name 프로퍼티는 인스턴스에 존재하므로 인스턴스를 가리켜야 하기 때문이다.

```js
// super 참조 의사 코드
// [[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
// [[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
super = Object.getPrototypeOf([[HomeObject]]);
```

```js
const obj = {
  foo() {}; // 메서드 축약 표현. [[HomeObject]]를 갖는다.
  bar: function () {} // 일반 함수.[[HomeObject]]를 갖지 않는다.
};

const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};

const derived {
  __proto__: base,
  // 메서드 축약 표현. [[HomeObject]]를 갖는다.
  sayHi() {
    return `${super.sayHi()}`;
  }
}
```

서브 클래스의 정적 메서드 내에서 super.sayHi는 super 클래스의 정적 메서드 sayHi를 가리킨다.

```js
class Base {
  static sayHi() {
    return "Hi";
  }
}

class Derived extends Base {
  static sayHi() {
    // super.sayHi는 super 클래스의 정적 메서드
    return `${super.sayHi()}. how are you doing?`;
  }
}

console.log(Derived.sayHi()); // Hi. how are you doing?
```

### 상속 클래스의 인스턴스 생성 과정

```js
class Rectangle {
  constuctor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

class ColorRectangle extends Rectangle {
  constuctor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, "red");
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

console.log(colorRectangle.getArea()); // 8
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

1. 서브클래스의 super 호출
   다른 클래스를 상속받지 않는 클래스는 내부 슬롯 [[ConstructorKind]] 값이 "base"
   다른 클래스를 상속받는 서브 클래스는 내부 슬롯 [[ConstructorKind]] 값이 "derived"

서브클래스는 자신이 직접 인스턴스를 생성하지 않고 super 클래스에 인스턴스 생성을 위임한다.
이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유다.

2. super클래스의 인스턴스 생성과 this 바인딩

super클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다.

이 빈 객체가 클래스가 생성한 인스턴스이며, 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.

```js
class Rectangle {
  console.log(this); // ColorRectangle {}
  console.log(new.target); // ColorRectangle
}
```

new.target은 서브클래스를 가리킨다.
인스턴스는 new.target이 가리키는 서브클래스가 생성된 것으로 처리된다.

```js
class Rectangle {
  constructor(width, height) {
    console.log(this); // ColorRectangle {}
    console.log(new.target) // ColorRectangle

    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
    console.log(this instanceOf ColorRectangle); // true
    console.log(this instanceOf Rectangle); // true

  }
}
```

3. super 클래스 인스턴스 초기화

super 클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

this에 바인딩 되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

```js
class Rectangle {
  constructor(width, height) {
    console.log(this);
    console.log(new.target);

    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype);
    console.log(this instanceOf ColorRectangle); // true
    console.log(this instanceOf Rectangle); // true

    this.width = width;
    this.height = height;

    console.log(this); // ColorRectangle {width: 2, height: 4}
  }
}
```

4. 서브클래스 constructor로의 복귀와 this 바인딩

super 호출이 종료되고 제어 흐름이 서브클래스 constructor로 돌아온다.

이때 super가 반환한 인스턴스가 this에 바인딩된다.

서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.

```js
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    console.log(this); // ColorRectangle {width: 2, height: 4}
  }
}
...
```

super가 호출되지 않으면 인스턴스가 생성되지 않으며 this 바인딩도 할 수 없다.

서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 이 때문이다.

5. 서브클래스의 인스턴스 초기화

super 호출 이후 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다.

this에 바인딩된 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스 프로퍼티를 초기화한다.

6. 인스턴스 반환

클래스의 모든 처리 후 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```js
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);

    console.log(this); // ColorRectangle {width: 2, height: 4}
  }
  this.color = color;
  console.log(this); // ColorRectangle {width: 2, height: 4, color: "red"}
}
...
```

### 표준 빌트인 생성자 함수 확장

extends 키워드 다음에는 클래스뿐 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.

> String, Number, Array 같은 표준 빌트인 객체도 extends 키워드로 확장할 수 있다.

```js
class MyArray extends Array {
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
console.log(myArray.average()); // 1.75

console.log(myArray instanceof MyArray); // true
console.log(myArray instanceof Array); // true
console.log(myArray instanceof Object); // true
console.log(myArray.uniq() instanceof MyArray); // true
console.log(myArray.uniq() instanceof Array); // true

// map, filter처럼 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다.
console.log(myArray.filter((v) => v % 2) instanceof MyArray); // true

// 만약 Array 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝이 불가하다.
console.log(
  myArray
    .filter((v) => v % 2)
    .uniq()
    .average()
); // 2
```

만약 MyArray 클래스의 uniq 메서드가 Array가 생성한 인스턴스를 반환하게 하려면 Symbol.species를 사용해 정적 접근자 프로퍼티를 추가한다.

```js
class MyArray extends Array {
  // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
  static get [Symbol.species]() {
    return Array;
  }

  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray instanceof MyArray); // true
console.log(myArray instanceof Array); // true
console.log(myArray instanceof Object); // true
console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

console.log(myArray.uniq().average()); // TypeError: myArray.uniq(...) average is not a function
```
