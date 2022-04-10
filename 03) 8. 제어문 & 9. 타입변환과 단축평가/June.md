# 8. 제어문

> 제어문은 조건에 따라 코드 블록을 실행하거나 반복 실행할 때 사용한다.

## 8.1 블록문

> 블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.

원래 문 끝에서는 세미콜론을 붙이는 게 일반적인데, 블록문은 문의 종료를 의미하는 자체 종결성을 갖기 때문에 세미콜론을 붙이지 않는다.

```javascript
// 블록문
{
  const hi = "hi";
}

// 제어문
let hi = "hi";
if (hi !== "hi") {
  hi = "hello";
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

## 8.2 조건문

### 8.2.1 if - else문

```javascript
// if - else문
if (조건식) {
  ...
} else {
  ...
}

// if - else if - else 문
if (조건식) {
  ...
} else if (조건식) {
  ...
} else {
  ...
}

// 보통 if - else 문은 3항연산자로 바꿔쓸 수 있다.
let result = "";
if (조건식) {
  result = "good";
} else {
  result = "bad";
}

result = 조건식 ? "good" : "bad";
```

### 8.2.2 switch문

```javascript
switch (표현식) {
  case 표현식1:
    // 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // 표현식과 표현식1이 일치하면 실행될 문;
    break;
  default:
  // 표현식과 일치하는 case문이 없을 때 실행될 문;
}
```

- `break`문을 써주지 않으면 의도치 않은 결과가 나올 수 있다.
- `default`뒤에는 `break`를 쓰지 않는다. (어차피 switch문이 끝나니까)
- `if - else`로 처리할 수 있으면 처리하는 게 낫고, 조건이 너무 많을 때는 `switch`를 사용하자.

## 8.3 반복문

- 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 이는 조건식이 거짓일 때 까지 반복된다.

- 자바스크립트는 세 가지 반복문인 `for`, `while`, `do...while`을 제공한다.

  - 배열을 순회할 때는 `forEach`
  - 객체의 프로퍼티를 열거할 때는 `for...in`
  - 이터러블을 순회할 수 있는 `for...of`
  - 이 처럼 자바스크립트에서 반복문을 대체할 수 있는 다양한 기능을 제공한다.

## 8.4 break문

- `break`문은 코드 블록을 탈출한다.
- 레이블 문, 반복문, switch문 이외에는 `break`를 사용하면 문법 에러가 발생한다.

```javascript
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log("foo");
```

- 중첩된 for문을 빠져나가고 싶을 때는 레이블 문을 사용하면 된다.

```javascript
// outer라는 식별자를 가진 레이블 for 문 생성
outer: for (let i = 0; i < 2; i++) {
  for (let j = 0; j < 2; j++) {
    // i + j가 2이면 바깥의 for문을 빠져나간다.
    if (i + j === 2) break outer;
    console.log(`${i}, ${j}`);
  }
}
```

- 근데 위의 레이블문은 위의 예시 이외에는 별로 권장되지 않는다.
- 가독성이 떨어지고, 오류를 발생시킬 가능성이 높아진다.

## 8.5 continue문

- continue문은 반복문의 코드 블록 실행을 현 지점에서 중단하고, 반복문의 증감식으로 실행 흐름을 이동시킨다. break 문처럼 반복문을 탈출하지는 않는다.

# 9. 타입 변환과 단축 평가

## 9.1 타입 변환이란?

- 자바스크립트에의 모든 값에는 타입이 있는데 개발자가 의도적으로 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅** 이라고 한다.
- 개발자가 의도적으로 변환하는 게 아니라 자바스크립트 엔진에 의해 변환되는 것을 **암묵적 타입 변환** 또는 **타입 강제 변환**이라 한다.
- 타입 변환이 기존 원시 값을 직접 변경하는 것은 아니다. 원시 값은 변경 불가능한 값이므로 변경할 수 없다.
- 명시적 타입 변환은 개발자가 대놓고 변환해서 알아보기 그나마 쉬운데, 암묵적으로 변할 때는 우리가 예측할 수 있어야한다.

### 9.2.3 불리언 타입으로 변환

- `Falsy`: false로 판단되는 값
  - false, undefined, null, 0, -0, NaN, ''(빈 문자열)
- `Truthy`: true로 판단되는 값
  - `Falsy` 이외에는 전부 true로 판별된다.

## 9.3 명시적 타입 변환

### 9.3.1 문자열 타입 변환

- String(), ''.toString()
- 혹은 문자열 연결 연산자 `+`를 이용해 암묵적 타입 변환

### 9.3.2 숫자 타입으로 변환

- Number(), parseInt(), parseFloat()
- +'0' 와 같이 단항 산술 연산자를 이용해 암묵적으로 변환
- \*'0' 와 같이 암묵적으로 변환

### 9.3.3 불리언 타입으로 변환

- Boolean()
- !!x 처럼 부정 논리 연산자를 두 번 사용

## 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가

- && 논리곱 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환
- || 논리합 연산자는 둘 중 하나만 true로 평가되어도 true를 반환

```javascript
true || anything; // true
false || anything; // anything
true && anything; // anything
false && anything; // false
```

### 9.4.2 옵셔널 체이닝 연산자

- ECMAScript2020에 도입
- 옵셔널 체이닝 연산자 `?`는 좌항의 피연산자가 null 또는 undefined일 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```javascript
var hyeonsu = "hyeonsu";

var value = hyeonsu?.old;
console.log(value); // undefined;
```

### 9.4.3 null 병합 연산자

- ECMAScript2020에 도입
- 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. 기본값 설정에 유용하다.

```javascript
var foo = null ? 'default string';
console.log(foo) // default string
```
