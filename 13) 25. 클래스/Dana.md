# 25. 클래스
## 25.1 클래스는 프로토타입의 문법적 설탕인가?
자바스크립트 = 프로토타입 기반 객체지향 언어
클래스 없이도 생성자 함수와 프로토타입을 통해 객체 지향 언어의 상속을 구현
```javascript=
var Person = (function() {
    // 생성자 함수
    function Person (name){
        this.name = name;
    }
    
    // 프로토타입 메서드
    Person.prototype.sayHi = function(){
        console.log('Hi, my name is '+ this.name)
    };
    
    // 생성자 함수 반횐
    return Person;
})

// 인스턴스 생성
var me = new Person('Lee');
me.sayHi();
```

클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하는 건 아님.
|차이점 | 클래스 | 생성자 함수 |
|------|-----|-------|
|new연산자 | 무조건 사용 | 없으면 일반 함수로 호출 |
|extend, super | 키워드 제공 | 키워드 제공 X|
|호이스팅 | X | 함수 선언문 - 함수 호이스팅, 함수 표현식 - 변수 호이스팅 |
|strict mode | 암묵적으로 설정 | X |
|Enumerable | 모두 false | |

## 25.2 클래스 정의
클래스는 class 키워드를 사용해 정의 - 보통 파스칼 케이스 사용

함수와 마찬가지로 표현식으로 정의할 수 있음.
이름을 가질 수도 있고, 갖지 않을 수도 있음
== **클래스**가 **값으로 사용할 수 있는 일급 객체**이다.
- 무명의 리터럴로 생성 가능 (런타임에 생성 가능)
- 변수나 자료구조에 저장 가능
- 함수의 매개변수에 전달 가능
- 함수의 반환값으로 사용 가능

클래스 몸체에서 정의할 수 있는 메서드
- constructor(생성자)
- 프로토타입 메서드
- 정적 메서드

## 25.3 클래스 호이스팅
클래스의 타입 = 함수

런타임 이전에 먼저 평가되어 함수 객체를 생성
클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수(contructor)

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문에 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨.

클래스는 클래스 정의 이전에 참조할 수 없어 호이스팅이 발생하지 않는 것처럼 보이나 호이스팅이 발생 (let, const의 호이스팅과 동일하게 동작) -> **모든 선언문은 런타임 이전에 먼저 실행되기 때문에**

## 25.4 인스턴스 생성
클래스 = 생성자 함수 ; new 연산자와 함께 호출되어 인스턴스를 생성

클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 무조건 new 연산자와 함께 호출해야함. 

기명함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능하기 때문에, 식별자를 통해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름을 이용해 인스턴스 생성시 에러 발생

## 25.5 메서드
클래스 몸체에서 정의할 수 있는 메서드
1. 생성자 (constructor)
2. 프로토타입 메서드
3. 정적 메서드

### 25.5.1 constructor
인스턴스를 생성하고 초기화하기 위한 특수한 메서드
```javascript=
class Person {
    // 생성자
    constructor(name){
        this.name = name;
    }
}
```

클래스는 평가되어 `함수 객체`가 됨. 함수 객체의 프로퍼티를 갖고 있으며, 프로토타입과 연결되어 있고 자신의 스코프체인을 구성

클래스가 constructor 메서드를 가지고 있는 것이 아님.
메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 되어 해당 동작을 하는 함수 객체가 생성

> 클래스의 constructor != 프로토타입의 constructor

#### constructor
- 클래스 내에 최대 한 개만 존재
- 생략 가능 -> 빈 contructor가 암묵적으로 정의
- 초기화된 인스턴스를 생성하려면 this를 추가
- 별도의 반환문을 갖지 않음
    - 명시적으로 반환시, 암묵적으로 this 반환
    - 반드시 return 생략

### 25.5.2 프로토타입 메서드
생성자 함수를 사용해 인스턴스를 생성하는 경우, 명시적으로 프로토타입에 메서드를 추가해야함.

```javascript=
function Person(name){
    this.name = name;
}

// 메서드 추가
Person.prototype.sayHi = function(){
    console.log(`hi, my name is ${this.name}`);
}

const me = new Person('kim');
```

하지만 클래스의 경우, 기본적으로 프로퍼타입 메서드로 지정됨.
```javascript=
function Person(name){
    this.name = name;
}

// 메서드 추가
sayHi(){
    console.log(`hi, my name is ${this.name}`);
}

const me = new Person('kim');
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토 타입 체인의 일원이 됨.
인스턴스는 프로토타입 메서드를 상속받아 사용 가능

프로토타입 체인은 기존의 모든 객체 생성방식 뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용

### 25.5.3 정적 메서드
정적 메서드 : 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

생성자 함수에서는 다음과 같이 지정
```javascript=
function Person(name){
    this.name = name;
}

// 메서드 추가
Person.sayHi = function(){
    console.log(`hi, my name is ${this.name}`);
}

const me = new Person('kim');
```

클래스에서는 `static`을 붙이면 정적 메서드가 됨.
```javascript=
function Person(name){
    this.name = name;
}

// 메서드 추가
static sayHi(){
    console.log(`hi, my name is ${this.name}`);
}

const me = new Person('kim');
```

정적 메서드는 인스턴스의 프로토타입에 존재하지 않기 때문에 인스턴스로 호출 할 수 없음. 

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이
1. 정적 메서드와 프로토타입 메서드는 자신이 속해있는 프로토타입 체인이 다름
2. 정적 메서드는 클래스로 호출, 프로토타입 메서드는 인스턴스로 호출
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티 참조 가능

-> 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용하며, 프로토타입 메서드로 정의해야함. this를 사용하는 메서드는 정적 메서드로 정의.

> 클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아놓으면 이름 충돌 가능성을 줄여주고 관련 함수를 구조화할 수 있는 효과가 있음

### 25.5.5 클래스에서 정의한 메서드의 특징
1. function 키워드를 생략한 메서드 축약 표현을 사용
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의시, 콤마가 필요없음
3. 암묵적으로 strict mode로 동작
4. for...in 문이나 Object.keys 메서드 등으로 열거 불가능
5. 내부 메서드 Contructor를 갖지 않아 New 연산자와 함께 호출 할 수 없음

## 25.6 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 this 바인딩
    - 내부 코드가 실행되기 앞서 빈 객체가 생성 (= 클래스가 생성한 인스턴스). 이 객체가 this에 바인딩 됨.
2. 인스턴스 초기화
    - this에 바인딩되어 있는 인스턴스를 초기화. 만약 constructor 생략 시 이 과정도 생략
3. 인스턴스 반환
    - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 This가 암묵적으로 반환

## 25.7 프로퍼티
### 25.7.1 인스턴스 프로퍼티
인스턴스 프로퍼티는 constructor 내부에서 정의해야하며 항상 public으로 존재

### 25.7.2 접근자 프로퍼티
자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수

값을 얻어오는 getter함수와 값을 수정하는 setter 함수로 구성

```javascript=
cosnt person = {
    
    // fullname은 접근자 함수로 구성된 접근자 프로퍼티
    get fullname() {
        ...
    }
        
    // 단 하나의 값만 할당받기 때문에 하나의 매개변수만 선언 가능
    set fullname(arg) {
        ...
    }
}
```

### 25.7.3 클래스 필드 정의 제안
클래스 필드 : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어

최신 문법으로 contructor 없이 클래스 내부의 변수를 선언할 수 있음
```javascript=
class Person {
    name = 'kim' (O)
    
    // 몸체에 바로 선언하는 경우에는 this 사용 불가
    this.name = 'kim' (X)
}
```
- 외부에서 초기값을 받아와야하는 경우 contructor를 사용해야함
- 몸체에 선언하는 경우 값을 반드시 할당해줘야함.
- 클래스 필드에 함수를 할당하는 경우, 함수는 **인스턴스의 프로퍼티**가 된다. (권장 X)

### 25.7.4 Private 필드 정의 제안
private 필드의 선두에 #을 붙여 사용
```javascript=
class Person {
    //private 필드 정의
    #name = '';
    
    constructor(name){
        this.#name = name;
    }
}

cosnt me = new Person('Lee');
console.log(me.#name)
```

#으로 선언된 필드는 접근자 프로퍼티를 통해서만 접근 가능

반드시 클래스 몸체에 정의해야함.

### 25.7.5 static 필드 정의 제안
```javascript=
class MyMath {
    // static public 필드 정의
    static PI = 22/7;
    
    // static private 필드 정의
    static #num = 10;
    
    static increment(){
        return ++MyMath.#num;
    }
}

console.log(MyMath.PI); // 3.14..
console.log(MyMath.increment()); // 11
```

## 25.8 상속에 의한 클래스 확장
### 25.8.1 클래스 상속과 생성자 함수 상속
상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

### 25.8.2 extends 키워드
상속을 통해 클래스를 확장하려면 extends 키워드를 사용해 상속받을 클래스를 정의

```javascript=
// super class
class Parents{}

// sub class
class Child extends Parents {}
```

슈퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐만 아니라 클래스간 프로토타입 체인도 생성 -> 프로토타입 메서드, 정적 메서드 모두 상속 가능

### 25.8.3 동적 상속
클래스가 생성자 함수를 포함한 contruct 내부 메서드를 갖는 함수 객체를 상속 받을 수도 있음
```javascript=
function Parents () {}

class Child extend Parents
```

이를 이용해 상속 받는 객체를 동적으로 정할 수 있음.

```javascript=
class Child extend (condition ? a : b)
```

### 25.8.4 서브클래스의 constructor 

클래스에서 constructor 생략시 비어있는 contructor가 암묵적으로 정의됨.

서브클래스에서 생략되는 경우 다음와 같은 contructor가 정의됨.
```javascript=
constructor(...args){
    super(...args)
}
```

### 25.8.5 super 키워드
super 키워드는 함수처럼 호출할 수도 있고, this와 같이 식별자처럼 참조할 수 있는 특수한 키워드
- super를 호출하면 수퍼클래스의 construcor를 호출
- super를 참조하면 수퍼클래스의 메서드를 호출


#### super 호출

수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 있음. 

호출 시 주의사항
1. 서브클래스에서 constructor를 생략하지 않는 경우, 서브클래스의 constructor에는 반드시 super를 호출해야한다.

2. 서브클래스의 contructor에서 super 호출 전까진 this를 참조할 수 없다.

3. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 contructor나 함수에 호출하면 syntax에러가 발생한다.

#### super 참조
1. 서브 클래스의 프로토타입 메서드 내에서 super.sayHi()는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다.

super 참조가 동작하기 위해서는 super를 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 찾을 수 있어야 함.
-> 이를 위해 메서드는 내부 슬롯 `[[HomeObject]]`를 가지며 자신을 바인딩하고 있는 객체를 가리킴

> ES6의 메서드 축약 표현으로 정의된 함수만이 HomeObject를 가짐

```javascript=
const obj = {
    // ES6의 메서드 축약 표현으로 정의한 메서드
    foo() {
    }
    
    // 일반 함수 -> HomeObject를 갖지 않음
    bar: function() {}
}
```

2. 서브클래스의 정적메서드 내에서 super.sayHi는 수퍼클래스의 정적메서드 sayHi를 나타낸다.

### 25.8.6 상속 클래스의 인스턴스 생성 과정
상속관계에 있는 두 클래스가 협력해 인스턴스를 생성하는 과정

서브클래스가 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스 생성

#### 1. 서브클래스의 super 호출

수퍼클래스와 서브클래스를 구분하기 위한 내부 슬록 `contructorKind`가 존재 - 다른 클래스를 상속받지 않는 클래스의 경우, `contructorKind == base` / 다른 클래스를 상속받는 경우, `contructorKind == derived`
    서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임 -> 서브클래스의 constructor에서 반드시 super를 호출해야하는 이유

#### 2. 수퍼클래스의 인스턴스 생성과 this 바인딩
수퍼클래스의 constuctor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성
    이 인스턴스는 this에 바인딩되는데, 인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리됨.
    따라서 생성된 인스턴스의 프로토타입은 수퍼클래스의 prototype 프로퍼티가 가리키는 객체가 아니라 new.target(서브클래스의 prototype 프로퍼티가 가리키는 객체)이다

#### 3. 수퍼클래스의 인스턴스 초기화
수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 받은 초기값으로 인스턴스의 프로퍼티를 초기화

#### 4. 서브클래스 contructor로의 복귀와 this 바인딩
super의 호출이 종료되고, 제어 흐름이 서브클래스로 돌아오면서 super가 반환한 인스턴스가 this에 바인딩
서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩해 그대로 사용
> 수퍼클래스의 인스턴스를 그대로 사용하기 때문에 super 호출 전까지 this를 참조할 수 없음

#### 5. 서브클래스의 인스턴스 초기화
super 호출 -> 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화 실행 -> 인스턴스에 프로퍼티 추가

#### 6. 인스턴스 반환
완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

### 25.8.7 표준 빌트인 생성자 함수 확장
extends 키워드 다음에 construct 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 식 사용 가능
-> 표준 빌트인 객체도 확장 가능

이 때 부모에게서 상속받은 메서드를 사용하더라도 해당 객체는 서브클래스의 인스턴스를 반환함.

만약 부모의 인스턴스를 반환하도록 하고 싶다면 `Symbol.species`를 사용해 정적 접근자 프로퍼티를 추가

```javascript=
class MyArray extends Array {
    static get [Symbol.species]() {return Array;}
    // 모든 메서드가 Array 타입의 인스턴스를 반환
    // 서브 클래스 메서드 간 메서드 체이닝이 되지 않음
}
```