# 32. String & 33. Symbol

## 32. String

### 32.1 String 생성자 함수

`String` 객체는 생성자 함수 객체다. 따라서 `new` 연산자와 함께 호출하여 `String` 인스턴스를 생성할 수 있다.

```js
const strObj = new String('Lee');
console.log(strObj);
// String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
```

`String` 객체는 유사 배열 객체이면서 이터러블이다. 따라서 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.

단, 문자열은 원시값이므로 변경할 수 없다. 그리고 이때 에러가 발생하지 않는다.

```js
strObj[0] = '5';
console.log(strObj); // 'Lee'
```

`String` 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, `[[StringData]]` 내부 슬롯에 변환된 문자열을 할당한 `String` 래퍼 객체를 생성한다.

```js
let strObj = new String(123);
console.log(strObj);
// String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123"}

strObj = new String(null);
console.log(strObj);
// String {0: "n", 1: "u", 2: "l", 3: "l", length: 4, [[PrimitiveValue]]: "null"}
```

`new` 연산자를 사용하지 않고 `String` 생성자 함수를 호출하면 `String` 인스턴스가 아닌 문자열을 반환한다. 이를 이용해 명시적으로 타입을 변환하기도 한다.

```js
// 숫자 타입 -> 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"

// 불리언 타입 -> 문자열 타입
String(true); // "true"
String(false); // "false"
```

### 32.2 length 프로퍼티

`length` 프로퍼티는 문자열의 문자 개수를 반환한다.

```js
'Hello'.length; // 5
'안녕하세요!'.length; // 6
```

### 32.3 String 메서드

`String` 객체에는 원본 `String` 래퍼 객체를 변경하는 메서드는 존재하지 않는다.
즉, `String` 객체의 메서드는 언제나 새로운 문자열을 반환한다.

문자열은 변경 불가능한 값이기 때문에 **String 래퍼 객체도 읽기 전용 객체로 제공된다.**

#### 32.3.1 String.prototype.indexOf

```js
const str = 'Hello World';

// 문자열 str에서 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('l'); // 2

// 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf('l', 3); // 3
```

#### 32.3.2 String.prototype.search

```js
const str = 'Hello world';

// 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
str.search(/o/); // 4
str.search(/x/); // -1
```

#### 32.3.3 String.prototype.includes

```js
const str = 'Hello World';

str.includes('Hello'); // true
str.includes('x'); // false
```

#### 32.3.4 String.prototype.startsWith

```js
const str = 'Hello World';

// 문자열 str이 'He'로 시작하는지 확인
str.startsWith('He'); // true
// 문자열 str이 'x'로 시작하는지 확인
str.startsWith('x'); // false
```

#### 32.3.5 String.prototype.endsWith

```js
const str = 'Hello World';

// 문자열 str이 'ld'로 끝나는지 확인
str.endsWith('ld'); // true
// 문자열 str이 'x'로 끝나는지 확인
str.endsWith('x'); // false
```

#### 32.3.6 String.prototype.charAt

```js
const str = 'Hello';

for (let i = 0; i < str.length; i++) {
 console.log(str.charAt(i)); // H e l l o
}

str.charAt(5); // ''
```

#### 32.3.7 String.prototype.substring

```js
const str = 'Hello World';

// 인덱스 1부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
str.subString(1, 4); // 'ell'

// 인덱스 1부터 마지막 문자까지 부분 문자열을 반환한다.
str.subString(1); // 'ello World'
```

#### 32.3.8 String.prototype.slice

```js
const str = 'Hello World';

// substring과 동일하게 동작한다.
str.slice(1, 4); // 'ell'

// substring과 동일하게 동작한다.
str.subString(1); // 'ello World'

// slice 메서드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환한다.
str.slice(-5); // 'World'
```

#### 32.3.9 String.prototype.toUpperCase

```js
const str = 'Hello World';

str.toUpperCase(); // 'HELLO WORLD'
```

#### 32.3.10 String.prototype.toLowerCase

```js
const str = 'Hello World';

str.toLowerCase(); // 'hello world'
```

#### 32.3.11 String.prototype.trim

```js
const str = '    foo    ';

str.trim(); // 'foo'
str.trimStart(); // 'foo    '
str.trimEnd(); // '    foo'
```

#### 32.3.12 String.prototype.repeat

```js
const str = 'abc';

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(2); // 'abcabc'
str.repeat(-1); // RangeError: Invalid cound value
```

#### 32.3.13 String.prototype.replace

```js
const str = 'Hello World World';

str.replace('World', 'Lee'); // 'Hello Lee World'

// $&는 검색된 문자열을 의미한다.
str.replace('World', '<strong>$&</strong>');
```

#### 32.3.14 String.prototype.split

```js
const str = 'How are you doing?';

str.split(' '); // ["How", "are", "you", "doing"]
str.split(''); // ["H", "o", "w", " ", .... "i", "n", "g"]
str.split(); //["How are you doing?"]
```

## 33. Symbol

### 33.1 심벌이란?

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.

따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

### 33.2 심벌 값의 생성

#### 33.2.1 Symbol 함수

심벌 값은 `Symbol` 함수를 호출하여 생성한다. 다른 원시값, 즉 문자열, 숫자, 불리언, `undefined`, `null` 타입의 값은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야 한다.

이때 생성된 심벌 값은 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```js
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

`Symbol` 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이는 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다.

```js
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다. 단, 불리언 타입으로는 암묵적으로 타입 변환된다.

```js
const mySymbol = Symbol();

console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a number

console.log(!!mySymbol); // true
```

#### 33.2.2 Symbol.for / Symbol.keyFor 메서드

`Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
- 검색에 실패하면 새로운 심벌 값을 생성하여 `Symbol.for` 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

`Symbol.keyFor` 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // undefined
```

### 33.3 심벌과 상수

```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

위에서 1, 2, 3, 4는 별로 의미가 없고, 상수 이름 자체에 의미가 있는 경우이다.

이러한 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

이를 좀더 확장시켜 다른 언어의 `enum`을 흉내 내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해 객체를 동결하는 `Object.freeze` 메서드와 심벌 값을 사용한다.

```js
// JavaScript enum
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

### 33.4 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있다.

심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // 1
```

심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

### 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for ... in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.

이처럼 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 ES6에 추가된 `Object.getOwnPropertySymbols` 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]
```

### 33.6 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

```js
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // 3
```

하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 앞으로도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```js
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```

### 33.7 Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 브라우저 콘솔에서 `Symbol` 함수를 참조하여 보자.

![Screenshot](https://velog.velcdn.com/images/dnr6054/post/34064884-140f-4f34-a1f5-639da7060600/image.png)

이 빌트인 심벌 값을 ECMAScript 사양에서는 `Well-known Symbol` 이라 부른다.

예를 들어 `Array`, `String`, `Map`, `Set`, `TypedArray`, `NodeList`, `HTMLCollection` 과 같이 `for ... of` 문으로 순회 가능한 빌트인 이터러블은 `Well-known Symbol` 인 `Symbol.iterator`를 키로 갖는 메서드를 가지며 `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.

```js
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      }
    };
  }
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```
