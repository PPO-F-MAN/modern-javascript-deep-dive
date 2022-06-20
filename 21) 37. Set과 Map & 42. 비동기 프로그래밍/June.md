# Set

> Set 객체는 중복되지 않는 유일한 값들의 집합이다. Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.

## Set 객체의 생성

```js
const set = new Set();
const set = new Set([1, 2, 3, 4]); // 이터러블을 받아 생성 가능
const set = new Set('Hello'); // 이터러블을 받아 생성 가능
```

## 요소 개수 확인

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size) // 3
```

## 요소 추가

```js
const set = new Set();
set.add(1)
```

- add 메서드는 연속적으로 호출 가능
- 중복된 요소는 추가되지 않는다 (에러는 나지 않음)

## 요소 존재 여부 확인

```js
const set = new Set([1,2,3]);

set.has(2); // true
set.has(4); // false
```

## 요소 삭제

```js
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set) // Set(2) [1, 3]
```

- 존재하지 않는 요소를 삭제하려하면 에러는 뜨지 않는다. (아무일도 일어나지 않음)
- 삭제 여부의 불리언 값을 반환한다.
- 연속적으로 호출할 수 없다.

## 요소 일괄 삭제

```js
set.clear();
```

## 요소 순회

- forEach 메서드를 사용한다.

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
```

- v와 v2는 똑같은 요소고, 배열의 forEach와 인터페이스를 같게 하기 위함이고 다른 이유는 없다.
- Set 객체는 이터러블이라서 for ... of 로 순회가능하고 스프레드 문법과 배열 디스트럭처링의 대상이될 수도 있다.

# Map

> Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. Map 객체는 객체와 유사하지만 다음과 같은 차이가 있다.

- 객체와 유사하지만 Map은 이터러블하고, 객체는 이터러블하지 않다.
- 객체는 length로 사이즈를 확인하고 Map은 size 메서드로 확인한다.

## Map 객체의 생성

> Map 생성자 함수는 이터러블 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```js
const map = new Map();

const map1 = new Map([['key1', 'value1'], ['key2', 'value2']])
```

## 요소 개수 확인

- size 메서드로 확인 가능

## 요소 추가

- set 메서드 사용

```js
map.set('key', 'value');
```

- set 메서드는 연속적으로 호출 가능하다.

## 요소 취득

- get 메서드 사용

```js
map.get('key');
```

## 요소 존재 여부 확인

- has 메서드 사용

```js
map.has('key');
```

## 요소 삭제

- delete 메서드 사용

```js
map.delete('key');
```

## 요소 일괄 삭제

- clear 메서드 샤용

## 요소 순회

- forEach 메서드 사용
- 첫 번째는 순회 중인 요소값, 두 번째 인수는 요소키, 세 번째 인수는 Map 객체 자체

```js
map.forEach((value, key, map) => console.log(value, key, map));
```

- Map 객체도 똑같이 이터러블이라서 for...of 사용가능, 스프레드 문법, 배열 디스트럭처링 할당 대상도 된다.

## 이터러블 메서드

- Map.keys()
- Map.values()
- Map.entries()

# 비동기 프로그래밍


