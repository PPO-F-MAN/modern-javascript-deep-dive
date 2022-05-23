# 28 Number

```javascript
/* Number 프로퍼티 */
Number.MAX_VALUE; // 자바스크립트에서 표현할 수 있는 가장 큰 수
Number.MIN_VALUE; // 자바스크립트에서 표현할 수 있는 가장 작은 수
Number.MAX_SAFE_INTEGER; // 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
Number.MIN_SAFE_INTEGER; // 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값
Number.POSITIVE_INFINITY; // 양의 무한대를 나타내는 Infinity와 같다.
Number.NEGATIVE_INFINITY; // 음의 무한대를 나타내는 Infinity와 같다.
Number.NaN; // window.NaN과 같다.

/* Number 메서드 */
Number.isFinite(); // 무한수인지 유한수인지 판별, 유한수면 true 반환
Number.isInteger(); // 정수면 true 반환
Number.isNaN(); // 숫자값이 NaN이면 true 반환
Number.isSafeInteger(); // 안전한 정수면 true, -(2^53 - 1), (2^53 - 1) 정수 값이다.
Number.prototype.toExponential(); // 지수 표기법으로 변환하여 문자열로 반환
Number.prototype.toFixed(); // 숫자를 반올림하여 문자열로 반환
Number.prototype.toPrecision(); // 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
Number.prototype.toString(); // 숫자를 문자열로 변환해서 반환한다.
```

# 29 Math

> 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다. Math는 생성자 함수가 아니라서 정적 프로퍼티와 정적 메서드만 제공한다.

```javascript
/* Math 프로퍼티 */
Math.PI; // 3.1415926535...

/* Math 메서드 */
Math.abs(); // 인수로 전달된 숫자의 절댓값을 반환한다.
Math.round(); // 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.
Math.ceil(); // 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.
Math.floor(); // 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.
Math.sqrt(); // 인수로 전달된 숫자의 제곱근을 반환한다.
Math.random(); // 0에서 1미만의 실수를 반환한다. 0은 포함, 1은 포함되지 않는다.
Math.pow(); // 첫 번째 인수는 밑, 두 번째 인수는 지수로 거듭제곱한 결과를 반환한다. 지수연산자 ** 랑 같다.
Math.max(); // 전달받은 인수 중에서 가장 큰 수를 반환한다.
Math.min(); //전달받은 인수 중에서 가장 작은 수를 반환한다.
```

# 30 Date

```javascript
Date.prototype.getDay(); // 요일을 나타내는 정수를 반환한다. 0부터 일요일
Date.prototype.toDateString(); // Fri Jul 24 2020 이런식으로 반환
Date.prototype.toTimeString(); // 12:300:00 GMT+0900 이런식으로 반환
Date.prototype.toISOString(); // 2020-07-24T03:30:00.000Z 이런식으로 반환, slice(0, 10)하면 날짜만 받아올 수 있음
```

### Date를 활요한 시계 예제

```javascript
(function printNow() {
  const now = new Date();

  const dayNames = [
    "(일요일)",
    "(월요일)",
    "(화요일)",
    "(수요일)",
    "(목요일)",
    "(금요일)",
    "(토요일)",
  ];

  const day = dayNames[now.getDay()];
  const year = now.getFullYear();
  const month = now.getMonth() + 1;
  const date = now.getDate();
  let hour = now.getHours();
  let minute = now.getMinutes();
  let second = now.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12로 변경

  // 10 미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const nowText = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

  console.log(nowText);

  setTimeout(printNow, 1000);
})();
```

# RegExp

> 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.

- /regexp/i 형식으로 입력한다. regexp 양쪽에는 시작, 종료 기호가 들어가고, 끝에 i 자리에는 플래그가 들어간다.

```javascript
/* RegExp 메서드 */
RedExp.prototype.exec; // 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해서 매칭 결과를 배열로 반환
RedExp.prototype.test; // 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해서 매칭 결과를 불리언 값으로 반환한다.
RedExp.prototype.match; // 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
```

### 플래그

> 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 총 6개인데 중요한 3개를 살펴본다.

- i (ignore case): 대소문자를 구별하지 않고 패턴을 검색
- g (global): 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색
- m (multi line): 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

### 패턴

```javascript
const regExp = /.../g; // 임의의 3자리 문자열을 대소문자 구별하여 전역 검색
const regExp = /A{1,2}/g; // A가 최소 1번, 최대 2번 반복되는 문자열 전역 검색
```

- `+`는 `{1,}`와 같다.
- A+ 는 'A'가 최소 한번 이상 반복되는 문자열, 즉 A만으로 이루어진 문자열과 매치한다.
- `?`는 최대 한 번 이상 반복되는 문자열, `?`는 `{0,1}`와 같다.
- `|`는 `or`과 같다. /A|B/는 'A'또는 'B'를 의미한다.
- `[]` 내에 `-`는 범위를 지정해준다. `[A-Z]` 대문자 알파벳을 검색한다.
- `\d`는 숫자를 의미한다.
- `[]` 안의 `^`은 `not`이다.
- `[]` 밖의 `^`은 해당 문자열로 시작하는지 판단한다.
- `$`은 문자열의 마지막을 의미한다.
