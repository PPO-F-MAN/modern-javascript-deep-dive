# String

## String 생성자 함수

```js
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}

const strObj = new String("Lee");
console.log(strObj); // String {0: "L", 1:"e", 2:"e", length: 3, [[PrimitiveValue]]: "Lee"}

// 명시적 타입 변환
String(1); // "1"
String(NaN); // "NaN" 
```

## length 프로퍼티

문자열의 문자 개수를 반환한다.

## String 메서드

문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다. (writable: false)

### String.prototype.indexOf

문자열을 검색해 첫 번째 인덱스를 반환
검색 실패하면 -1 반환
2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다.

### String.prototype.search

인수로 전달받은 정규 표현식과 매치하는 문자열을 검색해 일치하는 문자열의 인덱스를 반환
검색 실패하면 -1 반환

### String.prototype.includes

인수 문자열이 포함되어 있는지 확인 : true, false
2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다.

### String.prototype.startsWith

대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인 : true, false
2번째 인수로 검색을 시작할 인덱스를 지정할 수 있다.

### String.prototype.endsWith

대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인 : true, false
2번째 인수로 검색할 문자열의 길이 전달

### String.prototype.charAt

대상 문자열이 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환
범위를 벗어나면 빈 문자열 반환
String.prototype.charCodeAt, String.prototype.codePointAt

### String.prototype.substring

첫 번째 인수 인덱스 문자 ~ 두 번째 인수 인덱스 문자 바로 이전 문자까지 반환

### String.prototype.slice

String.prototype.substring과 동일하지만 음수 인수를 전달할 수 있다.

### String.prototype.toUpperCase

대문자 문자열 반환

### String.prototype.toLowerCase

소문자 문자열 반환

### String.prototype.trim

앞뒤 공백 문자 제거한 문자열 반환
String.prototype.trimStart, String.prototype.trimEnd

### String.prototype.repeat

대상 문자열을 인수만큼 반복

### String.prototype.replace

첫 번째 인수로 전달받은 문자열, 정규표현식을 검색해 두 번째 인수로 치환한 문자열을 반환
```js
const str = 'Hello world';
str.replace('world', '<string>$&</string>') // $& => 검색된 문자열
```

### String.prototype.split

첫 번째 인수 문자열 또는 정규 표현식을 검색해 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열 반환
두 번째 인수로 배열의 길이 지정

# Symbol

## 심벌이란?

변경 불가능한 원시 타입의 값
다른 값과 중복되지 않는 유일무이한 값이다.
유일한 프로퍼티 키를 만들기 위해 사용한다.

## 심벌 값의 생성

Symbol 함수를 호출해 생성한다.
다른 값과 절대 중복되지 않는 유일무이한 값이며 외부로 노출되지 않아 확인할 수 없다.

### Symbol 함수

```js
const mySymbol = Symbol();
new Symbol(); // TypeError: Symbol is not a constructor

const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false

const mySymbol = Symbol('mySymbol');
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)

console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a string

console.log(!!mySymbol); // true
if (mySymbol) console.log('mySymbol is not empty');
```

### Symbol.for / Symbol.keyFor 메서드

Symbol.for : 인수로 전달받은 문자열을 키로 사용해 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
- 검색에 성공하면 검색된 심벌 값 반환
- 실패하면 새로운 심벌 값을 생성해 저장한 후 생성된 심벌 값 반환

Symbol.keyFor : 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
const s1 = Symbol.for('mySymbol');
const s2 = Symbole.for('mySymbol');

console.log(s1 === s2); // true

Symbol.keyFor(s1); // mySymbol
```

### 심벌과 상수

```js
// enum을 흉내내어 객체의 변경 방지
const Direction = Object.freeze({
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGht: Symbol('right')
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

### 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며 동적으로 생성할 수 있다.
```js
const obj = {
  [Symbol.for('mySymbol')]: 1
};
obj[Symbol.for('mySymbol')]; // 1
```

### 심벌과 프로퍼티 은닉

심벌 값을 위처럼 프로퍼티로 생성하면 어느정도 은닉이 가능하다.

### 심벌과 표준 빌트인 객체 확장

표준 빌트인 객체는 중복을 방지해 읽기 전용으로 사용하는 것이 좋다.
심벌 값으로 표준 빌트인 객체를 확장하는 것이 좋다.

```js
Array.prototype[Symbol.for('sum')] = function() {
  return this.reduce((acc, cur) => arr + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```

### Well-known Symbol

빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.
이를 Well-known Symbol이라 부르며 자바스크립트 엔진의 내부 알고리즘에 사용된다.

예를 들어 순회 가능한 빌트인 이터러블은 Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며, Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다. 빌트인 이터러블은 이터레이션 프로토콜을 준수한다.

```js
const iterable = {
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
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