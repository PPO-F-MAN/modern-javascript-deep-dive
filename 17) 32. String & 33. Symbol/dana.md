# 32. String
## 32.1 string 생성자 함수
string 객체는 생성자 함수 객체
new 연산자와 함께 호출해 string을 생성할 수 있음.

인수를 전달하지 않고, new 연산자와 함께 호출하면 StringData 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성
```javascript=
const strObj = new String("test");
console.log(strObj); 
// String { length : 0, [[PrimitiveValue]] : ""}

const strObj = new String();
console.log(strObj); 
// String { 0 : 't', 1: 'e', 2 : 's', 3 : 't' , length : 0, [[PrimitiveValue]] : "test"}
```

PrimitiveValue === StringData (접근할 수 없는 프로퍼티)
length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블
따라서 인덱스를 사용해 접근할 수는 있지만, 인덱스를 이용해 값을 변경할 수는 없음.(readonly)

String에 인자로 문자열이 아닌 값을 넣으면 명시적으로 형 변환이 일어남.

## 32.2 length 프로퍼티
문자열의 문자 개수를 반환 하는 프로퍼티로, String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티를 가짐. 
1. 인덱스를 나타내는 숫자를 프로퍼티로 가짐
2. 각 문자를 프로퍼티 값으로 가짐
=> String 래퍼 객체는 유사 배열 객체

## 32.3 String 메서드 
배열에는 두가지 메서드가 존재
1. 원본 배열을 직접 변경하는 메서드 
2. 원본 배열을 직접 변경하지 않고 새로운 배열을 생성해 반환하는 메서드

**String 래퍼 객체를 직접 변경하는 메서드가 아니라 새로운 메서드를 생성해 반납하는 것**

### String.prototype.indexOf
> str.indexOf(검색할 값, [검색을 시작할 인덱스])

대상 문자열에서 인수로 전달받은 문자열을 검색해 첫 번째 인덱스를 반환, 실패시 -1 반환

특정 문자열이 존재하는지 확인할 때 유용하며, `String.prototype.includes` 메서드를 활용하면 더 좋음

### String.prototype.search
> str.search(정규표현식);

대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색해 일치하는 문자열의 인덱스를 반환, 실패시 -1 반환

### 32.3.3 String.prototype.includes
> str.includes(찾는 값, [검사를 시작할 인덱스])

대상 문자열에 인수로 전달 받은 문자열이 포함되어 있는지 확인해 결과를 boolean 값으로 반환

### 32.3.4 String.prototype.startsWith
> str.startsWith(찾는 값, [검색을 시작할 인덱스])

대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인해 그 결과를 boolean으로 반환

### 32.3.5 String.prototype.endsWith
> str.endsWith(찾는 값, [검색을 시작할 인덱스])

대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인해 그 결과를 boolean으로 반환

### 32.3.6 String.prototype.charAt
> str.charAt(인덱스 값)

대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색해 반환
인덱스는 문자열의 범위(0 ~ 문자열길이-1) 사이의 정수
인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환

유사한 메서드로 charCodeAt, codePointAt 이 있음.

### 32.3.7 String.prototype.substring
> str.substring(시작 문자 인덱스, [종료 문자 인덱스 + 1])

- 두번째 인자 생략시, 시작문자~마지막 인덱스까지
- 첫번째 인수 > 두번째 인수 => 두 인수가 교환
- 인수 < 0 || NaN => 0으로 취급
- 인수 > 문자열의 길이 => 인수는 문자열의 길이로 취급

indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득

### 32.3.8 String.prototype.slice
`substring` 메서드와 동일하게 동작하지만, 음수인 인수를 전달 할 수 있다는 점이 다름.
음수인 인자를 넣는 경우, 문자열의 가장 뒤에서부터 시작해 문자열을 잘라내 반환

### 32.3.9 String.prototype.toUpperCase
대상 문자열을 모두 대문자로 변경한 문자열을 반환

### 32.3.10 String.prototype.toLowerCase
대상 문자열을 모두 소문자로 변경한 문자열을 반환

### 32.3.11 String.prototype.trim
대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

`trimStart`나 `trimEnd`를 이용하면 앞 혹은 뒤의 공백만 제거 가능함

### 32.3.12 String.prototype.repeat
> str.repeat(정수)

대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열 반환
정수가 0이면 빈 문자열 반환, 음수면 RangeError 발생
인수 생략 시, 기본값 0이 설정

### 32.3.13 String.prototype.replace
> str.replace(문자열/정규표현식, 대체할 문자열)
- $&는 검색된 문자열을 의미 -> 해당 문자열에 다른 문자열을 추가하고 싶은 경우 사용

대상 문자열에서 첫번째 인수로 전달 받은 문자열 또는 정규표현식을 검색해 두번째 인수로 전달한 문자열로 치환한 문자열을 반환

두번째 인자로 치환 함수 전달 가능

### 32.3.14 String.prototype.split
> str.split(문자열/정규표현식, [배열의 길이])

대상 문자열에서 첫번째 인수로 전달한 문자열 또는 정규 표현식을 검색해 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환

빈 문자열을 인수로 전달 시, 각 문자를 모두 분리하고, 빈 인수를 전달하면 문장 전체를 단일 요소로 하는 배열을 반환

`reverse()`, `join()` 함수와 함께 문자열을 역순으로 뒤집을 수 있음.

# 33. 7번째 데이터 타입 Symbol
## 33.1 심벌이란?
ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
다른 값과 중복되지 않는 유일무이한 값
이름 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용
-> **하위 호환성을 보장하기 위해 도입**

## 33.2 심벌 값의 생성
### 33.2.1 Symbol 함수
Symbol 함수를 호출해 생성
이 때 생성된 심볼 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값

```javascript=
const mySymbol = Symbol()

// 심볼 값은 외부로 노출되지 않아 확인할 수 없음
console.log(mySymbol) // Symbol() 
```

다른 생성자 함수와 달리 new 연산자와 함께 호출하지 않음.

선택적으로 문자열을 인수로 전달 할 수 있으나. 심벌값에 대한 설명으로 디버깅 용도로 사용됨. 
-> 심벌 값에 대한 설명이 같더라도, 생성된 심볼은 유일무이한 값

심볼값도 객체처럼 접근시, 암묵적으로 래퍼 객체를 생성

```javascript=
cosnt mySymbol = Symbol('mySymbol');

console.log(mySymbol.description) // mySymbol
console.log(mySymbol.toString()) // Symbol(mySymbol)
```

심볼은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않음.
단, 불리언타입으로는 암묵적으로 변환 가능하다. -> 항상 true?

### 33.2.2 Symbol.for / Symbol.keyFor 메서드
#### Symbol.for 메서드
- 인수로 전달받은 문자열을 **키**로 사용해 키와 심볼 값의 쌍들이 저장되어 있는 전역 심볼 레지스트리에서 해당 키와 일치하는 심볼 값을 검색
- 검색 성공 시, 새로운 심볼 값을 생성하지 않고 검색된 심볼 값을 반환
- 검색 실패 시, 새로운 심볼 값을 생성해 Symbol.for 메서드의 인수로 전달된 키로 전역 심볼 레지스트리에 저장 후 생성된 심볼 값을 반환

`Symbol.for` 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심볼값을 단 하나만 생성해 전역 심볼 레지스트리를 통해 공유가 가능.

#### Symbol.keyFor 메서드
- 전역 심볼 레지스트리에 저장된 심볼 값의 키를 추출

## 33.3 심벌과 상수
예를 들어 방향을 나타내는 상수를 정의할 때,
```javascript=
cosnt Direction = {
    UP : 1,
    DOWN : 2,
    ...
}
```

**값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우**, 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심볼값 사용 가능

```javascript=
cosnt Direction = {
    UP : Symbol('up'),
    DOWN : Symbol('down'),
    ...
}
```

> ### enum
> 명명된 숫자 상수의 집합으로 열거형이라고 부름
> 자바스크립트에서는 enum을 사용하지 않지만, 타입스크립트에서는 enum을 지원
> 자바스크립트에서 enum을 흉내내기 위해 Object.freeze 메서드와 심벌값 사용
> ```javascript=
>const Direction = Object.freeze({
>    UP : Symbol('up'),
>    DOWN : Symbol('down'),
>    ...
>})
>```

## 33.4 심벌과 프로퍼티키
객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심볼 값으로 생성 가능 (동적으로도 생성 가능)

심볼값으로 프로퍼티 키를 동적 생성해 프로퍼티를 만드는 방법
```javascript=
const obj = {
    [Symbol.for('mySymbol')] : 1
}
```

심볼은 유일무이한 값이므로 심볼값으로 프로퍼티키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않음

## 33.5 심볼과 프로퍼티 은닉
심볼값을 프로퍼티 키로 사용해 생성한 프로퍼티는 `for...in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 검색 불가
-> 외부에 노출할 필요 없는 프로퍼티 은닉 가능

`Object.getOwnPropertySymbols` 메서드를 사용하면 심볼값을 키로 사용하는 프로퍼티 검색 가능

## 33.6 심벌과 표준 빌트인 객체(prototype) 확장
일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가해 확장하는 것은 권장하지 않음. 표준 빌트인 객체는 읽기 전용으로 사용하는 것을 권장

> why?
> 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문
> 중복될 가능성이 없는 심볼값으로 프로퍼티 키를 생성해 표준 빌트인 객체 확장시, 표준 빌트인 객체의 기존 프로퍼티와 충돌하지 않는 것은 물론, 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체 확장 가능

## 33.7 Well-known Symbol
자바스크립트가 기본 제공하는 빌트인 심볼 값 존재 -> 이를 well-known symbol 이라고 부름

만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면, 이터레이션 프로토콜을 따르면 됨.
- well-known symbol인 `Symbol.iterator`를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 됨.
```javascript=
const iterable = {
    [Symbol.iterator]() {
        let cur = 1;
        const max = 5;
        
        return {
            next() {
                return { value : cur ++ , done : cur >max + 1 };
            }
        }
    }
}
```
