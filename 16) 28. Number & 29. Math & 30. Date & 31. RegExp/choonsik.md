# Number

## Number 생성자 함수

```js
const numObj = new Number();
```

## Number 프로퍼티

### Number.EPSILON

1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이
2.22222 .... * 10^-16

부동소수점에 의한 오차를 해결하기 위해 사용

### Number.MAX_VALUE

Infinity 보다는 작은 가장 큰 양수 값

### Number.MIN_VALUE

0보다 크면서 가장 작은 양수 값

### Number.MAX_SAFE_INTEGER

안전하게 표현할 수 있는 가장 큰 정수값

### Number.MIN_SAFE_INTEGER

안전하게 표현할 수 있는 가장 작은 정수값

### Number.POSITIVE_INFINITY

Infinity

### Number.NEGATIVE_INFINITY

-Infinity

### Number.NaN

숫자가 아님

## Number 메서드

### Number.isFinite

Infinity가 아닌지 검사
NaN이 아닌지 검사

### Number.isInteger

정수인지 검사

### Number.isNaN

NaN인지 검사
전연 함수 isNaN과 다르게 암묵적 타입 변환이 없다

### Number.isSafeInteger

안전한 정수값인지

### Number.prototype.toExponential(e n제곱)

숫자 리터럴과 사용할 수 없다.
().~

### Number.prototype.toFixed(0)

숫자를 반올림해 문자열로 반환

### Number.prototype.toPrecision(0)

전체 자릿수 지정

### Number.prototype.toString(진수)

숫자를 문자열로 변환

# Math

## Math 프로퍼티

### Math.PI

원주율 값 반환

## Mate 메서드

### Math.abs

절대값 반환

### Math.round

소수점 이하를 반올림한 정수

### Math.ceil

올림

### Math.floor

내림

### Math.sqrt

제곱근 반환

### Math.random

0 <= x < 1

### Math.pow(밑, 지수)

거듭제곱

### Math.max

전달받은 인수 중 가장 큰 수

### Math.min

전달받은 인수 중 가장 작은 수

# Date

## Date 생성자 함수

UTC(GMT)

### new Date()

Date 객체 반환

### new Date(milliseconds)

해당 초 만큼 경과한 날짜와 시간 Date 객체 반환

### new Date(dateString)

지정된 날짜와 시간 Date 객체 반환
```js
new Date('May 26, 2020 10:00:00');
new Date('May/26/2020 10:00:00');
```

### new Date(year, month[, day, hour, minute, second, millisecond])

연, 월은 필수
```js
new Date('May', 26, 10, 00, 00, 0);
```

## Date 메서드

### Date.now

1970년 1월 1일 00:00:00(UTC) 기준 경과한 밀리초

### Date.parse

1970년 1월 1일 00:00:00(UTC) 기준에서 지정 시간까지 밀리초 반환

### Date.UTC

1970년 1월 1일 00:00:00(UTC) 기준 지정 시간까지 밀리초를 숫자로 반환
new Date(year, month[, day, hour, minute, second, millisecond]) 형식 인수

### Date.prototype.getFullYear

객체의 연도 정수 반환

### Date.prototype.setFullYear

객체의 연도 정수 설정

### Date.prototype.getMonth

월 반환 (0~11)

### Date.prototype.setMonth

월 설정 (0~11)

### Date.prototype.getDate

객체의 날짜 정수 반환

### Date.prototype.setDate

객체의 날짜 정수 설정

### Date.prototype.getDay

객체의 요일 정수 반환 (0 : 일요일)

### Date.prototype.getHours

객체의 시간 정수 반환 (0~23)

### Date.prototype.setHours

객체에 시간 정수 설정

### Date.prototype.getMinutes

객체의 분 정수 반환 (0~59)

### Date.prototype.setMinutes

객체의 분 정수 설정

### Date.prototype.getSeconds

객체의 초 정수 반환 (0~59)

### Date.prototype.setSeconds

객체의 초 정수 설정.

### Date.prototype.getMilliseconds

객체의 밀리초 반환 (0~999)

### Date.prototype.setMilliseconds

객체의 밀리초 설정

### Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC) 기준 경과된 밀리초 반환

### Date.prototype.setTime

1970년 1월 1일 00:00:00(UTC) 기준 경과된 밀리초 설정

### Date.prototype.getTimezoneOffset

UTC와 Date 객체 지정된 locale 시간 차이를 분 단위로 반환
KST : UTC + 9h
UTC = KST - 9h

### Date.prototype.toDateString

문자열로 날짜 반환

### Date.prototype.toTimeString

시간을 표현한 문자열 반환

### Date.prototype.toISOString

날짜와 시간을 표현한 문자열 반환

### Date.prototype.toLocaleString

locale 기준 날짜와 시간을 표현한 문자열 반환

### Date.prototype.toLocaleTimeString

local 기준 시간을 표현한 문자열 반환

## Date를 활용한 시계 예제

```js
(function printNow() {
  const today = new Date();

  const dayNames = [
    '(일요일)',
    '(월요일)',
    '(화요일)',
    '(수요일)',
    '(목요일)',
    '(금요일)',
    '(토요일)'
  ];
  // getDay 메서드는 해당 요일(0 ~ 6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];

  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? 'PM' : 'AM';

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? '0' + minute : minute;
  second = second < 10 ? '0' + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다. 41.2.1절 "setTimeout / clearTimeout" 참고
  setTimeout(printNow, 1000);
}());
```

# RegExp

## 정규 표현식이란?

Perl의 정규 표현식 문법
패턴 매칭 기능

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

## 정규 표현식의 생성

```js
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // -> true

const target = 'Is this all there is?';

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // -> true

const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); // -> 3
count('Is this all there is?', 'xx'); // -> 0
```

## RegExp 메서드

### RegExp.prototype.exec

매칭 결과를 배열로 반환
첫 번째 매칭 결과만 반환한다.

### RegExp.prototype.test

매칭 결과를 불리언 값으로 반환

### String.prototype.match

대상 문자열과 매칭 결과를 배열로 반환
모든 매칭 결과 반환

## 플래그

| 플래그 | 의미 | 설명 |
| - | - | - |
| i | Ignore case | 대소문자 구별 X 패턴 검색 |
| g | Global | 대상 문자열 내 전역 검색 |
| m | Multi line | 행이 바뀌더라도 패턴 검색 |

## 패턴

정규 표현식 : 형식 언어
```js
\패턴\
```

### 문자열 검색

exec, match, test

### 임의의 문자열 검색

. 사용

### 반복 검색

{m, n}
최소 m번 최대 n번
```js
+ = {1,}
? = {0, 1}
```

### OR 검색

```js
|
+ // 분해되지 않은 단어 레벨로 검색
[] // or로 동작
- // 범위 지정

const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]

const target = 'A AA BB ZZ Aa Bb';

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp = /[A-Z]+/g;

target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]

const target = 'AA BB Aa Bb 12';

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]

const target = 'AA BB 12,345';

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9]+/g;

target.match(regExp); // -> ["12", "345"]

const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9,]+/g;

target.match(regExp); // -> ["12,345"]

const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]

const target = 'Aa Bb 12,345 _$%&';

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " $%&"]
```

### NOT 검색

```js
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB ", " Aa Bb"]
```

### 시작 위치로 검색

```js
const target = 'https://poiemaweb.com';

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

### 마지막 위치로 검색

```js
const target = 'https://poiemaweb.com';

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

## 자주 사용하는 정규표현식

### 특정 단어로 시작하는지 검사

```js
const url = 'https://example.com';

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true

/^(http|https):\/\//.test(url); // -> true
```

### 특정 단어로 끝나는지 검사

```js
const fileName = 'index.html';

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

### 숫자로만 이루어진 문자열인지 검사

```js
const target = '12345';

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

### 하나 이상의 공백으로 시작하는지 검사

```js
const target = ' Hi!';

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
// \s = [\t\r\n\v\f]
```

### 아이디로 사용 가능한지 검사

```js
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

### 메일 주소 형식에 맞는지 검사

```js
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
```

### 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

### 특수 문자 포함 여부 검사

```js
const target = 'abc#123';

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
(/[^A-Za-z0-9]/gi).test(target); // -> true

(/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // -> true

target.replace(/[^A-Za-z0-9]/gi, ''); // -> abc123
```