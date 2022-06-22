# Set과 Map

## Set

Set 객체 : 중복되지 않는 유일한 값들의 집합
순서에 의미가 없고 인덱스로 요소에 접근할 수 없다.

### Set 객체의 생성

```js
const set = new Set();
console.log(set); // Set(0) {}
```

이터러블 인수

```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

중복 제거
```js
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 요소 개수 확인

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 요소 추가

```js
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

### 요소 존재 여부 확인

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 요소 삭제

```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

### 요소 일괄 삭제

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 요소 순회

```js
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3]
```

### 집합 연산

```js
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}

// or
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

```js
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}

// or 
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

```js
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}

// or
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false

// or
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## Map

키와 값의 쌍으로 이루어진 컬렉션
객체와 유사하지만 이터러블이고, 모든 값을 키로 사용할 수 있다.

### Map 객체의 생성

인수 이터러블은 키와 값의 쌍

```js
const map = new Map();
console.log(map); // Map(0) {}

const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

### 요소 개수 확인

```js
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```

### 요소 추가

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}
```

```js
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, 'value1').set(NaN, 'value2');
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, 'value1').set(-0, 'value2');
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```

### 요소 취득

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### 요소 존재 여부 확인

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

### 요소 삭제

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

### 요소 일괄 삭제

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

### 요소 순회

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]);
// [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
```

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```

# 비동기 프로그래밍

## 동기 처리와 비동기 처리

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다. 싱글 스레드 방식으로 동작한다.

```js
// sleep 함수는 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
function sleep(func, delay) {
  // Date.now()는 현재 시간을 숫자(ms)로 반환한다.("30.2.1. Date.now" 참고)
  const delayUntil = Date.now() + delay;

  // 현재 시간(Date.now())에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복한다.
  while (Date.now() < delayUntil);
  // 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
  func();
}

function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

// sleep 함수는 3초 이상 실행된다..
sleep(foo, 3 * 1000);
// bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 블로킹된다.
bar();
// (3초 경과 후) foo 호출 -> bar 호출
```

동기 처리는 실행 순서가 보장되지만 앞선 태스크 종료까지 태스크들이 블로킹되는 단점이 있다.

```js
function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

// 타이머 함수 setTimeout은 일정 시간이 경과한 이후에 콜백 함수 foo를 호출한다.
// 타이머 함수 setTimeout은 bar 함수를 블로킹하지 않는다.
setTimeout(foo, 3 * 1000);
bar();
// bar 호출 -> (3초 경과 후) foo 호출
```

비동기는 블로킹 없이 태스크를 곧바로 실행한다. 태스크 실행 순서가 보장되지 않는다.

비동기 함수는 콜백 패턴을 사용한다. 콜백 헬의 한계가 있어 예외 처리나 여러 비동기 처리에 한계가 있다.
타이머 함수인 setTImeout, setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.

## 이벤트 루프와 태스크 큐

자바스크립트의 동시성을 지원하는 것이 바로 이벤트 루프다.

콜 스택 : 스택 자료구조인 실행 컨텍스트 스택. 순차적으로 푸시되어 실행된다. 자바스크립트는 단 하나의 콜 스택을 사용하며 최상위 실행 컨텍스트가 종료되어 콜 스택에서 제거되지 전까지 다른 태스크가 실행되지 않는다.

힙 : 객체가 저장되는 메모리 공간. 콜 스택 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조한다. 메모리에 값을 저장하려면 메모리 공간의 크기를 결정해야 하는데, 객체는 런타임에 동적 할당되어야 한다. 
객체가 저장되는 힙은 구조화 되어 있지 않다.

비동기 처리에서 소스코드의 평가, 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저나 Node.js가 담당한다.

태스크 큐: setTImeout, setInterval 등 비동기 함수의 콜백 함수나 이벤트 핸들러가 일시적으로 보관되는 영역. 별도로 프로미스의 후속 처리 메서드의 콜백 함수가 일시적으로 보관되는 마이크로태스크 큐도 존재한다.

이벤트 루프 : 콜 스택에 실행 중인 실행 컨텍스트가 있는지, 태스크 큐에 대기 중인 함수가 있는지 반복해서 확인한다. 콜 스택이 비어있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킨다.

```js
function foo() {
  console.log('foo'); // (4)
}

function bar() {
  console.log('bar'); // (3)
}

setTimeout(foo, 0); // 0초(실제는 4ms) 후에 foo 함수가 호출된다. // (1)
bar(); // (2)
```

자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저는 멀티 스레드로 동작한다.
브라우저는 자바스크립트 엔진 외에도 렌더링 엔진과 Web API를 제공한다.
Web API는 DOM API, 타이머 함수, HTTP 요청과 같은 비동기 처리를 포함한다.