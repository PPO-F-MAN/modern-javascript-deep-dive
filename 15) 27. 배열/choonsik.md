# 배열

## 배열이란

여러 개의 값을 순차적으로 나열한 자료구조

요소 : 배열이 가지고 있는 값
인덱스 : 요소에 접근할 정수
length 프로퍼티 : 요소의 개수. 배열의 길이

생성 방식 : 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드

| 구분 | 객체 | 배열 |
| - | - | - |
| 구조 | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조 | 프로퍼티 키 | 인덱스 |
| 값의 순서 | X | O |
| length 프로퍼티 | X | O |

## 자바스크립트 배열은 배열이 아니다

희소 배열 : 배열의 요소가 연속적으로 이어져 있지 않은 배열
자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.

자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며 length 프로퍼티를 갖는다.
자바스크립트 배열의 요소는 사실 프로퍼티 값이다.

- 일반적인 배열은 인덱스로 요소에 빠르게 접근 가능하나 요소를 삽입 삭제하는 경우 효율적이지 않다.
- 자바스크립트 배열은 해시 테이블로 구현된 객체로, 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능에서 느리다. 하지만 요소의 삽입 삭제가 일반적인 배열보다 빠르다.

## length 프로퍼티와 희소 배열

length 프로퍼티 값을 할당하면 그 크기 만큼 배열의 길이가 줄어들 수 있다. 늘어날 수는 없다.
자바스크립트는 희소 배열을 문법적으로 허용한다.

희소 배열은 length와 배열 요소의 개수가 일치하지 않는다.
희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.

배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다.

## 배열 생성

### 배열 리터럴

가장 간편한 방식

```js
const arr= [1, , 3];
```

### Array 생성자 함수

```js
const arr = new Array(10); // 희소 배열
const arr = new Array(); // []
const arr = new Array(1, 2, 3); // [1, 2, 3]
const arr = new Array({}); // [{}]
```

- 전달 인수는 0 ~ 4,294,967,295

### Array.of

```js
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of('string'); // ['string']
```

### Array.from

유사 배열 객체, 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

```js
Array.from({length: 2, 0: 'a', 1: 'b'}); // ['a', 'b']
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
Array.from({ length: 3 }); // [undefined, undefined, undefined]
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

## 배열 요소의 참조

대괄호 표기법. 인덱스를 사용한다.
존재하지 않은 요소 접근은 undefined 반환

## 배열 요소의 추가와 갱신

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.
이미 존재하는 인덱스를 사용해 값을 할당하면 요소 값이 갱신된다.
정수 이외의 값을 인덱스처럼 사용하면 프로퍼티가 생성되며 length에 영향을 주지 않는다.

## 배열 요소의 삭제

```js
delete arr[1];
```

```js
arr.splice(삭제 시작할 인덱스, 삭제할 요소 수); // 권장
```

## 배열 메서드

Array 생성자 함수는 정적 메서드를, 
Array.prototype은 프로토타입 메서드를 제공한다.

배열에는 원본 배열을 직접 변경하는 메서드와 새로운 배열을 생성해 반환하는 메서드가 있다.
가급적이면 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.

### Array.isArray

전달된 인수가 배열이면 true, 아니면 false

### Array.prototype.indexOf

특정 요소가 존재하는지 확인
권장 : Array.prototype.includes()

### Array.prototype.push

원본 배열을 변경
권장 : arr[arr.length] = value;
권장 : const newArr = [...arr, 3];

### Array.prototype.pop

원본 배열을 변경
비어있으면 undefined

### Array.prototype.unshift

전달 인수를 모두 원본 배열의 선두에 추가하고 length 반환

### Array.prototype.shift

원본 배열 변경
첫 번째 요소를 제거하고 그 요소를 반환

### Array.prototype.concat

전달 인수를 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환
권장 : 스프레드 문법으로 대체할 수 있다.

### Array.prototype.splice(start, deleteCount, items)

원본 배열 변경

### Array.prototype.slice(start, end)

새로운 배열 반환

### Array.prototype.join

전달 인수를 구분자로 연결한 문자열을 반환한다.

### Array.prototype.reverse

원본 배열을 변경

### Array.prototype.fill(item, startIndx, endIndex)

원본 배열을 변경

### Array.prototype.includes(item, startIndex)

특정 요소 포함 여부

### Array.prototype.flat(depth)

배열 평탄화

## 배열 고차 함수

고차 함수 : 함수를 인수로 전달받거나 함수를 반환하는 함수. 불변성을 지향하는 함수형 프로그래밍 기반
함수형 프로그래밍 : 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제해 상태 변경을 피하려는 프로그래밍 패러다임. 순수 함수를 통해 부수 효과를 최대한 억제해 프로그램의 안정성을 높인다.

### Array.prototype.sort(비교 함수)

배열의 요소 정렬
숫자 타입도 일시적으로 문자열로 변환해 정렬한다.

quicksort 알고리즘에서 timsort 알고리즘으로 바뀌었다.

### Array.prototype.forEach

for문을 대체
반환값은 언제나 undefined. this도 undefined (strict mode)
break, continue문을 사용할 수 없다.
for문에 비해 성능은 떨어지나 가독성은 더 좋다.

### Array.prototype.map

콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.

### Array.prototype.filter(item, index, arr)

콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.

### Array.prototype.reduce(accumulator, currentValue, index, array)

인수로 전달된 콜백 함수를 반복 호출해 하나의 결과값을 반환한다.

```js
const average = values.reduce((acc, cur, i, {length}) => {
  return i === length - 1? (acc + cur) / length : acc + cur;
}, 0)

const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0)

const count = fruits.recude((acc, cur) => {
  acc[cur] = (acc[cur] || 0) + 1;
  return acc;
}, {})

const flatten = values.reduce((acc, cur) => acc.concat(cur), [])

const uniques = values.reduce((unique, val, i, _values) => 
  _values.indexOf(val) === i ? [...unique, val] : unique, []
)
```

### Array.prototype.some

콜백 함수에 대해 단 하나라도 참이면 true

### Array.prototype.every

콜백 함수에 대해 모두 참이면 true

### Array.prototype.find

콜백 함수의 반환값이 true인 첫번째 요소 or undefined

### Array.prototype.findIndex

콜백 함수를 호출해 반환값이 true인 첫번째 요소의 인덱스 or -1

### Array.prototype.flatMap

map, flat 메서드를 순차적으로 실행하는 효과