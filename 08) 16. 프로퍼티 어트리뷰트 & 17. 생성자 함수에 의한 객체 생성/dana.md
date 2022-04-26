# 16. 프로퍼티 어트리뷰트
## 16.1 내부 슬롯과 내부 메서드
내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 pseudo property와 pseudo method
이중 대괄호로 감싼 이름들이 내부 슬롯과 메서드 ([[]])

자바스크립트의 내부 로직으로 원칙적으로 개발자가 접근하거나 호출할 수 없음. 예를 들어 `[[Prototype]]` 이라는 내부 슬롯을 갖을 때 원칙적으로 접근 불가하지만, `__proto__` 를 통해 간접적으로 접근 가능

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 **프로퍼티 어트리뷰트**를 기본값으로 자동 정의

### **프로퍼티의 상태 / `[[프로퍼티 어트리뷰트]]`**

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값
- 프로퍼티의 값 `[[Value]]`
- 값의 갱신 가능 여부 `[[Writable]]`
- 열거 가능 여부 `[[Enumerable]]`
- 재정의 가능 여부 `[[Configurable]]`

직접 접근할 수는 없지만 `Object.getOwnPropertyDescriptor(객체의 참조,프로퍼티 키)` 메서드를 사용해 간접적으로 확인 가능

-> Property Descriptor 객체를 반환
-> 존재하지 않는 경우 undefined 반환
-> 모든 프로퍼티를 확인하고 싶다면 `Object.getOwnPropertyDescriptors(객체의 참조)`를 사용

## 16.3 데이터 프로퍼티와 접근자 프로퍼티
### **데이터 프로퍼티**
키와 값으로 구성된 일반적인 프로퍼티

### **접근자 프로퍼티**
자체적으로 값을 갖지 않고 다른 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티
#### `[[Value]]`
- 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값
- 프로퍼티 키를 변경시 `[[Value]]`에 값을 재할당. 이 때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 `[[Value]]`에 값을 저장

#### `[[Writable]]`
- 프로퍼티 값의 변경 가능 여부를 나타내며, 불리언 값을 가짐
- 값이 false인 경우, 해당 프로퍼티의 value는 읽기 전용 프로퍼티

#### `[[Enumerable]]`
- 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 가짐
- false인 경우, 해당 프로퍼티는 for...in문이나 Object.keys메서드 등 사용 불가

#### `[[Configurable]]`
- 프로퍼티 재정의 가능 여부를 나타내며 불리언 값을 가짐
- false인 경우, 해당 프로퍼티의 삭제, attribute의 값의 변경 금지
- 단 writable이 true인 경우, value의 변경과 writable를 false로 변경하는 것은 허용

### 16.3.2 접근자 프로퍼티
#### `[[Get]]`
- 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수
- getter함수가 호출되고 그 결과가 프로퍼티 값으로 반환

#### `[[Set]]`
- 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수
- setter함수가 호출되고 그 결과가 프로퍼티 값으로 저장

#### `[[Enumerable]]`
- 데이터 프로퍼티와 같음

#### `[[Configurable]]`
- 데이터 프로퍼티와 같음

```javascript=
const person = {
    // 데이터 프로퍼티 firstName: 'Ungmo' ,
    lastName: 'Lee' ,

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
    // getter 함수
    get fullName() {
        return `${this . firstName} ${this . lastName}`;
    } ,

    // setter 함수
    set fullName(name) {
    // 배열 디스트럭처링 할당: "31.1 배열 디스트럭처링 할당" 참고
    [this . firstName , this . lastName] = name . split(' ');
    }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console . log(person . firstName + ' ' + person . lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장 
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person . fullName = 'Heegun Lee';

console . log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조 
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티다.
// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 
// 프로퍼티 어트리뷰트를 갖는다.
let descriptor = Object . getOwnPropertyDescriptor(person , 'firstName');
console . log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 
// 프로퍼티 어트리뷰트를 갖는다.
descriptor = Object . getOwnPropertyDescriptor(person , 'fullName');
console . log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}

```


접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하는 경우,
`[[Get]]` 내부 메서드가 호출되어 다음과 같이 동작
1. 프로퍼티 키가 유효한지 확인 - 프로퍼티 키는 문자열 또는 심볼
2. 프로토타입 체인에서 프로퍼티 검색
3. 검색된 fullName 프로퍼티가 프로퍼티 어트리뷰트 `[[Get]]`의 값인 getter함수를 호출해 그 결과를 반환

접근자 프로퍼티와 데이터 프로퍼티를 구분하는 방법
-> `Object.getOwnPropertyDescriptor`의 프로퍼티가 다름

## 프로퍼티 정의
새로운 프로퍼티를 추가하며넛 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것

### Object.defineProperty
프로퍼티의 어트리뷰트를 정의하는 메서드
인수로 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체 전달

```javascript=
const person = {};

Object.defineProperty(person, 'firstName', {
    value : "어쩌구",
    writable : true,
    enumerable : true,
    configurable : true
})
```

descriptor 객체의 프로퍼티를 누락시키면 undefined, false가 기본값.
값이 false인 경우 각각

`writable` : 값 변경시 에러는 발생하지 않고 무시됨.

`enumerable` : 열거되지 않고, \["value값"] 출력

`configurable` : 해당 프로퍼티 삭제 불가능. 삭제시 무시됨. 재정의 시도시 TypeError

Object.defineProperty 사용시 값 생략 가능
|프로퍼티 디스크립터 객체의 프로퍼티  | 생략했을 때의 기본값 | 
|------|------|
|value | undefined |
|get | undefined |
|set | undefined |
|writable | false |
|enumerable | false |
|configurable | false |

한번에 하나의 프로퍼티만 정의할 수 있기 때문에 여러개의 프로퍼티를 한 번에 정의하고 싶다면 Object.defineProperties 메서드 사용

## 16.5 객체 변경 방지
|구분 | 메서드 | 추가 | 삭제 | 값 읽기 | 값 쓰기 | 어트리뷰트 재정의 |
|----|------|------|----|-----|----|-----|
|객체 확장 금지 | Object.preventExtensions | :x: | :heavy_check_mark: |:heavy_check_mark: |:heavy_check_mark: |:heavy_check_mark: |
|객체 밀봉 | Object.seal | :x: | :x: | :heavy_check_mark: |:heavy_check_mark: | :x: |
|객체 동결 | Object.freeze | :x: | :x: | :heavy_check_mark: | :x: | :x: |

### 16.5.1 객체 확장 금지
- 확장이 금지된 객체는 프로퍼티 추가가 금지됨.
- 확장이 가능한 객체인지 여부는 `Object.isExtensible` 메서드로 확인 가능

```javascript=
// person 객체는 확장이 금지된 객체가 아니다.
console . log(Object . isExtensible(person)); // true
// person 객체의 확장을 금지하여 프로퍼티 추가를 금지한다.
Object . preventExtensions(person);
// person 객체는 확장이 금지된 객체다.
console . log(Object . isExtensible(person)); // false
```

### 16.5.2 객체 밀봉
- 프로퍼티 추가 및 삭제가 불가능
- 프로퍼티 재정의 금지
- 읽기와 쓰기만 가능
- 밀봉된 객체 여부는 `Object.isSealed` 메서드로 확인 가능
- configurable == false

```javascript=
// person 객체는 밀봉(seal)된 객체가 아니다.
console . log(Object . isSealed(person)); // false
// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.
Object . seal(person);
// person 객체는 밀봉(seal)된 객체다.
console . log(Object . isSealed(person)); // true
```

### 16.5.3 객체 동결
- 프로퍼티 추가 및 삭제 불가능
- 어트리뷰트 재정의 금지
- 값 갱신 금지
- 읽기만 가능
- 동결된 객체 여부는 `Object.isFrozen` 메서드로 확인 가능
- writable == false
- configurable == false

```javascript=
// person 객체는 동결(freeze)된 객체가 아니다.
console . log(Object . isFrozen(person)); // false
// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지한다.
Object . freeze(person);
// person 객체는 동결(freeze)된 객체다.
console . log(Object . isFrozen(person)); // true
```

중첩 객체까지 동결해 불변 객체를 만들기 위해선 재귀적으로 Object.freeze메서드를 호출해야함.

# 17. 생성자 함수에 의한 객체 생성
## 17.1 Object 생성자 함수
```javascript=
const person = new Object(); // 빈 객체를 생성해 반환

person.name = 'Kim' // 프로퍼티를 동적으로 추가
```

> ### 생성자 함수
> new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
> 생성자 함수에 의해 생성된 객체를 인스턴스라고 함
> 자바스크립트에서는 String, Number, Date, RegExp 등 빌트인 생성자 함수를 제공

특별한 이유가 없다면 별로 유용하지 않은 생성 방법

## 17.2 생성자 함수
### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점
객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성
동일한 프로퍼티를 갖는 객체를 여러개 생성해야하는 경우 비효율적

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점
객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용해 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성 가능
```javascript=
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    };
}

const circle1 = new Circle(2); // 반지름이 5인 Circle 객체 생성
```

> ### this
> 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수
> 함수 호출 방시겡 따라 동적으로 결정됨.
> 일반 함수로서 호출 - 전역객체
> 메서드로서 호출 - 메서드를 호출한 객체
> 생성자 함수로서 호출 - 생성자 함수가 생성할 인스턴스

자바스크립트에서는 일반 함수와 동일한 방법으로 생성자 함수를 정의하기 때문에 new 연산자와 함께 호출하는 경우에만 새로운 인스턴스를 생성
-> new 연산자가 없는 경우, 일반 함수로 취급

### 17.2.3 생성자 함수의 인스턴스 생성 과정
생성자 함수의 역할
- 템플릿에 맞는 인스턴스 생성 (필수)
- 생성된 인스턴스를 초기화 (옵션)
- 암묵적으로 인스턴스를 생성하고 반환

#### 1. 인스턴스 생성과 this 바인딩
암묵적으로 빈 객체가 생성된 뒤, this에 바인딩

#### 2. 인스턴스 초기화
생성자 함수에 기술된 코드가 한 줄씩 실행되면, this에 바인딩 되어있는 인스턴스를 초기화

#### 3. 인스턴스 반환
생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
만약 명시적으로 다른 객체를 return 하면 this 반환이 무시되고 return한 객체가 반환됨. 따라서 생성자 함수 내부에서 return 객체를 사용하면 안됨

### 17.2.4 내부 메서드
함수는 객체지만 일반 객체와 다른 특징을 가짐
-> 일반 객체는 호출할 수 없지만 함수는 호출 가능
+ 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드
+ 함수 객체만을 위한 내부 슬롯 (`[[Environment]]`, `[[FormalParameters]]`)
+ 함수 객체만을 위한 내부 메서드 (`[[Call]]`, `[[Construct]]`)
    - 일반 함수로서 호출 시 Call 호출
    - new 연산자와 함께 호출 시 Construct 호출

내부 메서드 Call을 갖는 함수 객체 = callable -> 함수
내부 메서드 Contruct를 갖는 함수 객체 = constructor -> 생성자 함수로서 호출할 수 있는 함수


함수 객체는 반드시 callable
모든 함수는 Call을 갖지만, Construct를 갖지 않을 수 있음
-> 생성자 함수가 될 수 있냐 없냐의 차이

### 17.2.5 constructor와 non- constructor의 구분
`constructor` : 함수 선언문, 함수 표현식, 클래스
`non-constructor` : 메서드, 화살표 함수

프로퍼티의 값으로 할당된 것은 일반함수로 정의된 함수 - 메서드 :x: 
함수의 정의 방식에 따라 constructor인지 아닌지 결정됨.

```javascript=
const foo(){
    x : function() {} // constructor
    y() {} // non-constructor
}
```

non-constructor를 생성자 함수로 호출 시 TypeError 발생

### 17.2.6 new 연산자
new 연산자와 함께 호출 -> `[[construct]]` 호출
- this == 호출한 생성자 함수가 생성할 인스턴스

new 연산자 없이 호출 -> `[[call]]` 호출
- this == 전역 객체

### 17.2.7 new.target
생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위함.
this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역변수와 같이 사용 (메타 프로퍼티)

new 연산자와 함께 생성자 함수로서 호출되면 new.target은 자기 자신
-> :x: : `undefined`

```javascript=
// 생성자 함수
function Circle(radius) {
// 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
if (!new . target) {
// new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
return new Circle(radius);
}
...
}
```

> 스코프 세이프 생성자 패턴
> new.target을 사용할 수 없는 상황에서 사용 (ex. IE에서 사용)
> 
> if (!(this instanceof Circle)) {
> return new Circle(radius)
>}

new 연산자와 함께 생성된 객체는 프로토타입에 의해 생성자 함수와 연결됨.
대부분의 빌트인 생성자 함수에서는 이를 확인해 적절한 값을 반환하며,
String, Number와 같은 생성자 함수는 new 연산자가 없는 경우 타입 변환을 위해 사용됨.










