# DOM

**브라우저의 렌더링 엔진**은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 **DOM**을 생성한다.

> **DOM**(Document Object Model)은 **HTML 문서의 계층적 구조와 정보**를 표현하며 
이를 제어할 수 있는 **API**, 즉 **프로퍼티와 메서드를 제공하는 트리 자료구조**다. 

- DOM은 nodes와 objects로 문서를 표현한다. 
- DOM은 웹 페이지의 객체 지향 표현이며, 자바스크립트와 같은 스크립팅 언어를 이용해 DOM을 수정할 수 있다.
- API (web or XML page) = DOM + JS (scripting language)

## 노드

### HTML 요소와 노드 객체

**HTML 요소**
HTML 문서를 구성하는 개별적인 요소. 
렌더링 엔진에 의해 파싱되어 DOM(DOM 트리)을 구성하는 **요소 노드 객체**로 변환된다. 

HTML요소의 어트리뷰트 => 어트리뷰트 노드
HTML요소의 텍스트 콘텐츠 => 텍스트 노드
> 공백 텍스트 노드 : HTML 요소 사이의 개행이나 공백은 텍스트 노드가 된다.

**트리 자료구조**
노드들의 계층적 구조를 표현하는 비선형 자료구조

최상위 노드 : 루트 노드
리프 노드 : 자식 노드가 없는 노드

### 노드 객체 타입

DOM은 **노드 객체의 계층적인 요소**로 구성되며, 노드 객체는 종류가 있고 상속 구조를 갖는다.

노드 객체는 총 12개의 종류가 있는데,,

> Node.ELEMENT_NODE - 1
Node.ATTRIBUTE_NODE - 2
Node.TEXT_NODE - 3
Node.CDATA_SECTION_NODE - 4
Node.ENTITY_REFERENCE_NODE - 5
Node.ENTITY_NODE - 6
Node.PROCESSING_INSTRUCTION_NODE - 7
Node.COMMENT_NODE - 8
Node.DOCUMENT_NODE - 9
Node.DOCUMENT_TYPE_NODE - 10
Node.DOCUMENT_FRAGMENT_NODE - 11
Node.NOTATION_NODE - 12

다음 중요한 4가지를 살펴보자

1. **문서 노드**
  **document 객체**. DOM 트리의 최상위에 존재하는 루트 노드
  브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체
  전역 객체 window의 document 프로퍼티에 바인딩 되어 있다.

  ```js
  window.document
  document
  ```

  브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유한다.

  **HTML 문서당 document 객체는 유일하다.**

  document 객체는 DOM 트리 노드에 접근하기 위한 **진입점** 역할을 한다.

2. 요소 노드
HTML 요소를 가리키는 객체. 문서의 구조를 표현한다.

3. 어트리뷰트 노드
HTML 요소의 어트리뷰트를 가리키는 객체
어트리뷰트가 지정된 HTML 요소의 요소 노드에만 연결되어 있다. (요소 노드의 형제 노드가 아니다)

4. 텍스트 노드
HTML 요소의 텍스트를 가리키는 객체. 문서의 정보를 표현한다.
요소 노드의 자식 노드이며 리프 노드이다.

그 외에도 Comment 노드, DocumentType 노드, DocumentFragment 노드 등이 있다.

### 노드 객체의 상속 구조

DOM API를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색하고 
자신의 어트리뷰트와 텍스트를 조작할 수 있다.

DOM을 구성하는 노드 객체는 **호스트 객체**이자 **자바스크립트의 객체**이다. 
프로토타입에 의한 상속 구조를 갖는다. 

- 노드 객체는 Object - EventTarget - Node 인터페이스를 상속받는다.
- 문서 노드는 Document - HTMLDocument 인터페이스를 상속받고
- 어트리뷰트 노드는 Attr
- 텍스트 노드는 CharacterData 인터페이스를 각각 상속받는다.
- 요소 노드는 Element 인터페이스를 상속받으며 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스를 상속받는다.

**프로토타입 체인 관점**

예를 들어, 
input 요소 노드 객체는 HTMLInputElement, HTMLElement, Element, Node, EventTarget, Object의 prototype에 바인딩되어 있는 프로토타입 객체를 상속받아 프로토타입 체인에 있는 모든 프로토타입 프토퍼티나 메서드를 상속받아 사용할 수 있다.

> 노드 객체에는 노드 객체의 종류(노드 타입)에 상관없이 모든 노드 객체가 공통으로 갖는 기능이 있고, 고유한 기능도 있다.

| input 요소 노드 객체의 특성 | 프로토타입을 제공하는 객체 | 기능 |
| - | - | - |
| 객체 | Object | |
| 이벤트를 발생시키는 객체 | EventTarget | 이벤트 관련 기능(EventTarget.addEventListener, EventTarget.removeEventListener 등) | 
| 트리 자료구조의 노드 객체 | Node | 트리 탐색 기능(Node.parentNode, Node.childNodes, Node.previousSibling, Node.nextSibling 등)이나 노드 정보 제공 기능(Node.nodeType, Node.nodeName 등) |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체 | HTMLElement | style 프로퍼티 | 
| HTML 요소 중에서 input 요소를 표현하는 객체 | HTMLInputElement | value 프로퍼티 |

> DOM은 노드 객체의 종류(타입)에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 
이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 요소 노드 취득

HTML의 구조나 내용, 스타일 등을 동적으로 조작하려면 먼저 **요소 노드**를 취득해야 한다.

- 텍스트 노드는 요소 노드의 **자식** 노드
- 어트리뷰트 노드는 요소 노드와 **연결**되어 있다.

### id를 이용한 요소 노드 취득

`Document.prototype.getElementById()` : 인수로 전달한 id 어트리뷰트 값을 갖는 **하나**의 요소 노드를 탐색해 반환한다.

id는 HTML 문서 내 유일한 값이어야 하며, class 어트리뷰트와 달리 공백 문자로 구분해 여러 개의 값을 가질 수 없다. (하지만 HTML 문서 내에 중복된 id 값을 갖는 여러 개의 요소가 존재할 수 있고, 이 경우 첫 번째 요소 노드만 반환한다.)

### 태그 이름을 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementByTagName()`
인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색해 반환한다.

`getElementsByTagName()`은 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체 **HTMLCollection 객체**를 반환한다.
`getElementsByTagName('*');` // 모든 요소 노드를 탐색해서 반환한다. 

```js
// DOM 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
const $elems = document.getElementsByTagName('li');

// 취득한 요소는 다음처럼 HTMLCollection 객체를 배열로 변환해 순회할 수 있다.
[...$elems].forEach(elem => {elem.style.color = 'red';});

// ul#parent 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
const $parent = document.getElementById('parent');
const $child = $parent.getElementsByTagName('li');

// 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.
```

> HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.

### class를 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementByClassName()`
인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색해 반환한다.
인수로 여러 개의 class를 공백으로 구분해 지정할 수 있다.
여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체 **HTMLCollection 객체**를 반환한다.

```js
// DOM 전체에서 클래스 값이 banana인 요소 노드를 모두 탐색하여 반환한다.
const $elems = document.getElementsByClassName('banana');

// ul#parent 요소의 자손 노드 중에서 클래스 값이 banana인 요소 노드를 모두 탐색하여 반환한다.
const $parent = document.getElementById('parent');
const $child = $parent.getElementByClassName('banana');

// 존재하지 않는 경우 빈 HTMLCollection 객체를 반환한다.
```

### CSS 선택자를 이용한 요소 노드 취득

> CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

```css
* { ... } /* 전체 선택자 */
p { ... } /* 태그 선택자 */
#foo { ... } /* 아이디 선택자 */
.foo { ... } /* class 선택자 */
input[type=text] { ... } /*  어트리뷰트 선택자 */
div p { ... } /* 후손 선택자 */
div > p { ... } /*  자식 선택자 */
p + ul { ... } /*  인접 형제 선택자 */
p ~ ul { ... } /*  일반 형제 선택자 */
a:hover { ... } /* 가상 클래스 선택자 */
p::before { ... } /* 가상 요소 선택자 */
```

`Document.prototype/Element.prototype.querySelector()`
인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색해 반환한다.
- 만족시키는 요소 노드가 여러 개면 첫 번째 요소 노드만 반환
- 존재하지 않는 경우 null 반환
- 문법에 맞지 않는 경우 DOMException 에러

`Document.prototype/Element.prototype.querySelectorAll()`
인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 **NodeList 객체**로 반환한다.

> NodeList 객체는 유사 배열 객체이면서 이터러블이다.

- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
- 문법에 맞지 않는 경우 DOMException 에러
- 인수로 '*' 전달하면 전체 선택

> `querySelector`, `querySelectorAll` 메서드는 `getElementBy***`, `getElementsBy***`보다 느리다고 알려져 있지만, 좀 더 **구체적**으로 요소 노드를 취득할 수 있고 **일관성** 있다는 장점이 있다.
따라서 id 어트리뷰트가 있는 경우 `getElementById`를 사용하고 그 외에는 `querySelector`, `querySelectorAll` 메서드를 사용할 것을 권장한다.

### 특정 요소 노드를 취득할 수 있는지 확인

`Element.prototype.matches`
인수로 전달한 CSS 선택자를 통해 특정 노드를 취득할 수 있는지 확인한다.
이벤트 위임을 사용할 때 유용하다.

```js
const $apple = document.querySelector('.apple');
console.log($apple.matches('#fruits > li.apple'));
```

### HTMLCollection과 NodeList

- DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체
- 유사 배열 객체이면서 이터러블 (for ... of문으로 순회할 수 있고 스프레드 문법을 사용해 배열로 변환할 수 있다.)
- 노드 객체의 상태 변화를 실시간으로 반영하는 live 객체
- HTMLCollection은 늘 live 객체로 동작하지만 NodeList의 경우 non-live 객체로 동작하기도 한다.

#### HTMLCollection
노드 객체의 상태 변화를 실시간으로 반영하는 live DOM 컬렉션 객체

```html
<!DOCTYPE html>
<head>
  <style>
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<html>
  <body>
    <ul id="fruits">
      <li class="red">Apple</li>
      <li class="red">Banana</li>
      <li class="red">Orange</li>
    </ul>
    <script>
      // class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('red');
      // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
      console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

      // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
      for (let i = 0; i < $elems.length; i++) {
        $elems[i].className = 'blue';
      }

      // HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
      console.log($elems); // HTMLCollection(1) [li.red]
    </script>
  </body>
</html>
```

1. i = 0일 때 : $elems에서 첫번째 Apple이 실시간으로 삭제된다.
2. i = 1일 때 : $elems에서 두번째 Orange가 실시간으로 삭제된다.
3. i = 2일 때 : $elems.length가 1이므로 반복이 종료된다.

다음과 같이 해결할 수 있다.

```js
// for 문을 역방향으로 순회
for (let i = $elems.length - 1; i >= 0; i--) {
  $elems[i].className = 'blue';
}

// while 문으로 HTMLCollection에 요소가 남아 있지 않을 때까지 무한 반복
let i = 0;
while ($elems.length > i) {
  $elems[i].className = 'blue';
}

// 유사 배열 객체이면서 이터러블인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach(elem => elem.className = 'blue');
```

#### NodeList
HTMLCollection 객체의 부작용을 해결하기 위해 `getElementsBy***` 대신 `querySelectorAll`을 사용할 수 있다.
이 때의 NodeList 객체는 non-live 객체이다.

```js
// querySelectorAll은 DOM 컬렉션 객체인 NodeList를 반환한다.
const $elems = document.querySelectorAll('.red');

// NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
$elems.forEach(elem => elem.className = 'blue');
```

NodeList.prototype은 forEach, item, entries, keys, values 메서드를 제공한다.

NodeList 객체는 대부분 non-live 객체로 동작하지만, 
childNodes 프로퍼티가 반환하는 NodeList 객체는 live 객체로 동작하므로 주의해야 한다.

```js
const $fruits = document.getElementById('fruits');

// childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
const { childNodes } = $fruits;
console.log(childNodes instanceof NodeList); // true
console.log(childNodes); // NodeList(5) [text, li, text, li, text]

for (let i = 0; i < childNodes.length; i++) {
  // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM에서 삭제한다.
  // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes가 실시간으로 변경된다.
  // 따라서 첫 번째, 세 번째 다섯 번째 요소만 삭제된다.
  $fruits.removeChild(childNodes[i]);
}

// 예상과 다르게 $fruits 요소의 모든 자식 노드가 삭제되지 않는다.
console.log(childNodes); // NodeList(2) [li, li]
```

> 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션은 사용하려면 HTMLCollection이나 NodeList 객체를 **배열로 변환**해 사용(스프레드 문법, Array.from 메서드)하는 것을 권장한다.

## 노드 탐색

DOM 트리 노드를 이리저리 옮겨 다니며 부모, 형제, 자식 노드를 탐색할 때

Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

- Node.prototype : parentNode, previousSibling, firstChild, childNodes 프로퍼티
- Element.prototype : previousElementSibling, nextElementSibling, children

노드 탐색 프로퍼티는 읽기 전용 접근자 프로퍼티이다.

### 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다.

### 자식 노드 탐색

| 프로퍼티 | 설명 |
| - | - |
| Node.prototype.childNodes | 자식 노드를 모두 탐색해 NodeList에 담아 반환한다. 요소노드, 텍스트 노드를 포함한다. |
| Element.prototype.children | 자식 노드 중에서 요소 노드만 모두 탐색해 HTMLCollection에 담아 반환한다. 텍스트 노드가 포함되지 않는다. |
| Node.prototype.firstChild | 첫 번째 자식 노드 반환. 텍스트 노드이거나 요소 노드다. |
| Node.prototype.lastChild | 마지막 자식 노드 반환. 텍스트 노드이거나 요소 노드다. |
| Element.prototype.firstElementChild | 첫 번째 자식 요소 노드 반환 |
| Element.prototype.lastElementChild | 마지막 자식 요소 노드 반환 |

```js
// 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
const $fruits = document.getElementById('fruits');

console.log($fruits.childNodes);
// NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

console.log($fruits.children);
// HTMLCollection(3) [li.apple, li.banana, li.orange]

console.log($fruits.firstChild); // #text

console.log($fruits.lastChild); // #text

console.log($fruits.firstElementChild); // li.apple

console.log($fruits.lastElementChild); // li.orange
```

### 자식 노드 존재 확인

Node.prototype.hasChildNodes
텍스트 노드를 포함해 자식 노드의 존재를 확인한다.
```js
const $fruits = document.getElementById('fruits');

console.log($fruits.hasChildNodes()); // true

// 요소 노드만 확인하려면
console.log(!!$fruits.children.length); // 0 -> false
console.log(!!$fruits.childElementCount); // 0 -> false
```

### 요소 노드의 텍스트 노드 탐색

firstChild로 접근할 수 있다.
```html
<!DOCTYPE html>
<html>
<body>
  <div id="foo">Hello</div>
  <script>
    // 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
    console.log(document.getElementById('foo').firstChild); // #text
  </script>
</body>
</html>
```

### 부모 노드 탐색

Node.prototype.parentNode
```js
console.log($banana.parentNode); // ul#fruits
```

### 형제 노드 탐색

| 프로퍼티 | 설명 |
| - | - |
| Node.prototype.previousSibling | 부모가 같은 자신의 이전 형제 노드. 요소 노드 또는 텍스트 노드 |
| Node.prototype.nextSibling | 부모가 같은 자신의 다음 형제 노드. 요소 노드 또는 텍스트 노드 |
| Element.prototype.previousElementSibling | 부모가 같은 자신의 이전 형제 요소 노드 |
| Element.prototype.nextElementSibling | 부모가 같은 자신의 다음 형제 요소 노드 |

## 노드 정보 취득

| 프로퍼티 | 설명 |
| - | - |
| Node.prototype.nodeType | 노드 객체의 종류. 상수 반환 |
| | Node.ELEMENT_NODE: 요소 노드 타입. (1) 반환 |
| | Node.TEXT_NODE: 텍스트 노드 타입. (3) 반환 |
| | Node.DOCUMENT_NODE: 문서 노드 타입. (9) 반환 |
| Node.prototype.nodeName | 노드 이름을 문자열로 반환 |
| | 요소 노드 : 대문자 문자열 태그 이름("UL", "LI") 반환 |
| | 텍스트 노드 : 문자열 "#text" 반환 |
| | 문서 노드 : 문자열 "#document" 반환 |

```js
console.log(document.nodeType); // 9
console.log(document.nodeName); // #document

const $foo = document.getElementById('foo');
console.log($foo.nodeType); // 1
console.log($foo.nodeName); // DIV

const $textNode = $foo.firstChild;
console.log($textNode.nodeType); // 3
console.log($textNode.nodeName); // #text
```

## 요소 노드의 텍스트 조작

### nodeValue
참조와 할당이 모두 가능한 프로퍼티

참조 시 노드 객체의 값(텍스트 노드의 텍스트)을 반환하거나 null을 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 문서 노드의 nodeValue 프로퍼티를 참조한다.
    console.log(document.nodeValue); // null

    // 요소 노드의 nodeValue 프로퍼티를 참조한다.
    const $foo = document.getElementById('foo');
    console.log($foo.nodeValue); // null

    // 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
    const $textNode = $foo.firstChild;
    console.log($textNode.nodeValue); // Hello

    // 값 변경
    $textNode.nodeValue = 'World';
    console.log($textNode.nodeValue); // World
  </script>
</html>
```

### textContent

요소 노드의 텍스트와 자손 노드의 텍스트를 모두 취득하거나 변경한다.
이때 HTML 마크업은 무시된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    console.log(document.getElementById('foo').textContent); // Hello world!

    // 이런 경우 nodeValue는 더 복잡하다.
    console.log(document.getElementById('foo').nodeValue); // null
    console.log(document.getElementById('foo').firstChild.nodeValue); // Hello
    console.log(document.getElementById('foo').lastChild.firstChild.nodeValue); // world!
  </script>
</html>
```

> innerText 프로퍼티는 textContent와 유사하게 동작하지만 다음 이유로 권장하지 않는다.
1. CSS 순종적. visibility: hidden 요소 노드의 텍스트를 반환하지 않는다.
2. CSS를 고려해 textContent 프로퍼티보다 느리다.

## DOM 조작

새로운 노드를 생성해 DOM에 추가하거나 기존 노드를 삭제/교체하는 것
이때 리플로우와 리페인트가 발생해 성능에 영향을 준다.

### innerHTML

Element.prototype.innerHTML setter, getter 모두 존재하는 접근자 프로퍼티
요소 노드의 HTML 마크업을 취득하거나 변경한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.
    console.log(document.getElementById('foo').innerHTML);
    // "Hello <span>world!</span>"
  </script>
</html>
```

textContent와 달리 HTML 마크업을 제거하지 않는다.
innerHTML 프로퍼티에 문자열을 할당하면 포함된 HTML 마크업이 파싱되어 DOM에 반영된다.

> 사용자로부터 입력받은 데이터를 innerHTML 프로퍼티에 할당하는 것은 XSS(Cross-Site Scripting Attacks)에 취약해 위험하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 에러 이벤트를 강제로 발생시켜서 자바스크립트 코드가 실행되도록 한다.
    document.getElementById('foo').innerHTML
      = `<img src="x" onerror="alert(document.cookie)">`;
  </script>
</html>
```

HTML Sanitization으로 잠재적 위험을 제거할 수 있다.
DOMPurify 라이브러리 사용을 권장한다.
```js
DOMPurity.sanitize('<img src="x" onerror="alert(document.cookie)">');
// => <img src="x">
```

또 다른 단점으로 요소 노드의 기존의 모든 자식 노드를 재 생성해 반영할 수 있다는 문제가 있다.
또한 새로운 요소 삽입 시 위치를 지정할 수 없다는 단점도 있다.

### insertAdjacentHTML 메서드

`Element.prototype.insertAdjacentHTML(position, DOMString)` 메서드는 기존 요소를 제거하지 않고 위치를 지정해 새로운 요소를 삽입한다.
innerHTML 보다 효율적이고 빠르나, XSS에 취약한 점은 동일하다.

HTML 마크업 문자열 DOMString을 파싱하고 생성된 노드를 position에 삽입해 DOM에 반영한다.
첫 번째 인수로 전달할 수 있는 문자열 : beforebegin, afterbegin, beforeeend, afterend

`beforebegin <div>afterbegin 텍스트 beforeend</div> afterend`

```html
<!DOCTYPE html>
<html>
  <body>
    <!-- beforebegin -->
    <div id="foo">
      <!-- afterbegin -->
      text
      <!-- beforeend -->
    </div>
    <!-- afterend -->
  </body>
  <script>
    const $foo = document.getElementById('foo');

    $foo.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>');
    $foo.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>');
    $foo.insertAdjacentHTML('beforeend', '<p>beforeend</p>');
    $foo.insertAdjacentHTML('afterend', '<p>afterend</p>');
  </script>
</html>
```

### 노드 생성과 추가

```js
const $fruits = document.getElementById('fruits');

// 1. 요소 노드 생성
const $li = document.createElement('li');

// 2. 텍스트 노드 생성
const textNode = document.createTextNode('Banana');

// 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
$li.appendChild(textNode);

// 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
$fruits.appendChild($li);
```

1. 요소 노드 생성
Document.prototype.createElement(tagName) : 요소 노드를 생성하여 반환한다.

2. 텍스트 노드 생성
Document.prototype.createTextNode(text) : 텍스트 노드를 생성하여 반환한다.

3. 텍스트 노드를 요소 노드의 자식 노드로 추가
Node.prototype.appendChild(childNode) : 노드의 마지막 자식으로 인수 노드를 추가한다.

요소 노드에 자식 노드가 하나도 없는 경우에는 그냥 textContent를 사용하면 편하다.
단 요소 노드에 자식 노드가 있으면 자식 노드가 모두 제거될 수 있다.

4. 요소 노드를 DOM에 추가
Node.prototype.appendChild를 사용해 DOM에 추가한다. 
이때 리플로우와 리페인트가 발생한다.

### 복수의 노드 생성과 추가

DOM 변경은 높은 비용이 드는 처리이므로 가급적 횟수를 줄이는 편이 성능에 유리하다.
기존 DOM에 반복적으로 추가하는 것은 비효율적이다.

> DocumentFragment : 부모 노드가 없어 기존 DOM과 별도로 존재한다. 별도의 서브 DOM을 구성해 기존 DOM에 추가하는 용도로 사용한다.
DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다.

`Document.prototype.createDocumentFragment`

### 노드 삽입

#### 마지막 노드로 추가
`Node.prototype.appendChild` : 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.

#### 지정한 위치에 노드 삽입
`Node.prototype.insertBefore(newNode, childNode)` : newNode를 childNode 앞에 삽입한다.
childNode는 반드시 호출한 노드의 자식 노드여야 한다. 
childNode가 null이면 appendChild처럼 동작한다.

### 노드 이동

DOM에 이미 존재하는 노드를 `appendChild`, `insertBefore`을 사용해 이동시킬 수 있다.

### 노드 복사

`Node.prototype.cloneNode([deep: true|false])` : 노드의 사본을 생성해 반환한다.
얕은 복사의 경우 자손 노드를 복사하지 않아 텍스트 노드가 없다.

### 노드 교체

`Node.prototype.replaceChild(newChild, oldChild)` : oldChild를 newChild로 교체한다.
당연히 oldChild 호출한 노드의 자식 노드여야 한다. oldChild 노드는 DOM에서 제거된다.

### 노드 삭제

`Node.prototype.removeChild(child)` : child 노드를 DOM에서 삭제한다.
당연히 child 노드는

## 어트리뷰트

### 어트리뷰트 노드와 attributes 프로퍼티

HTML 요소는 여러 개의 어트리뷰트(속성)를 가질 수 있다.
HTMl 요소의 시작 태그에 `어트리뷰트 이름="어트리뷰트 값"` 형식으로 정의한다.

```html
<input id="user" type="text" value="ungmo2">
```

- 글로벌 어트리뷰트 : id, class, style, title, lang, tabindex, draggable, hidden
- 이벤트 핸들러 어트리뷰트 : onclick, onchange, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover, onsubmit, onload

id, class, style은 모든 HTML 요소에 쓸 수 있지만 type, value, checked는 input 요소에만 쓸 수 있다.

HTML 요소가 파싱될 때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다.
모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 `NameNodeMap` 객체에 담겨 요소 노드의 attributes 프로퍼티에 저장된다.

어트리뷰트 노드는 요소 노드의 `Element.prototype.attributes` 프로퍼티(일기 전용 접근자)로 취득할 수 있다.

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    // 요소 노드의 attribute 프로퍼티는 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.
    const { attributes } = document.getElementById('user');
    console.log(attributes);
    // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

    // 어트리뷰트 값 취득
    console.log(attributes.id.value); // user
    console.log(attributes.type.value); // text
    console.log(attributes.value.value); // ungmo2
  </script>
</body>
</html>
```

### HTML 어트리뷰트 조작

`Element.prototype.getAttribute/setAttribute` : attributes 프로퍼티를 통하지 않고 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있다.

`Element.prototype.hasAttribute(attributeName)` : 특정 HTML 어트리뷰트가 존재하는지 확인
`Element.prototype.removeAttribute(attributeName)` : 특정 HTML 어트리뷰트 삭제

### HTML 어트리뷰트 vs DOM 프로퍼티

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(DOM 프로퍼티)가 존재한다.
DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있으며 참조와 변경이 가능하다.

**HTML 어트리뷰트**
1. 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드 - 초기 상태를 관리
2. HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티(DOM 프로퍼티) - 최신 상태를 관리

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // attributes 프로퍼티에 저장된 value 어트리뷰트 값
    console.log($input.getAttribute('value')); // ungmo2

    // 요소 노드의 value 프로퍼티에 저장된 value 어트리뷰트 값
    console.log($input.value); // ungmo2
  </script>
</body>
</html>
```

#### 어트리뷰트 노드
HTML 요소의 초기 상태. 상태에 따라 변하지 않는다.
getAttribute/setAttribute

#### DOM 프로퍼티
사용자가 입력한 최신 상태. 상태 변화에 반응한다.

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // DOM 프로퍼티에 값을 할당하여 HTML 요소의 최신 상태를 변경한다.
    $input.value = 'foo';
    console.log($input.value); // foo

    // getAttribute 메서드로 취득한 HTML 어트리뷰트 값, 즉 초기 상태 값은 변하지 않고 유지된다.
    console.log($input.getAttribute('value')); // ungmo2
  </script>
</body>
</html>
```

사용자 입력과 관계없는 id 어트리뷰트와 id 프로퍼티는 항상 동일한 값으로 연동된다.

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // id 어트리뷰트와 id 프로퍼티는 사용자 입력과 관계없이 항상 동일한 값으로 연동한다.
    $input.id = 'foo';

    console.log($input.id); // foo
    console.log($input.getAttribute('id')); // foo
  </script>
</body>
</html>
```

#### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

- id 어트리뷰트와 id 프로퍼티는 1:1 대응. 동일한 값으로 연동
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응. 
value 어트리뷰트는 초기 상태를, value 프로퍼티는 최신 상태
- class 어트리뷰트는 className, classList 프로퍼티와 대응
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응
- td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 없다.
- textContent 프로퍼티는 대응하는 어트리뷰트가 없다.
- 어트리뷰트는 대소문자 구별 X. 대응하는 프로퍼티 키는 카멜 케이스

#### DOM 프로퍼티 값의 타입
getAttribute : 문자열
DOM 프로퍼티 : 문자열이거나 불리언이거나.. 

### data 어트리뷰트와 dataset 프로퍼티

data 어트리뷰트, dataset 프로퍼티를 사용하면 HTML 요소에 정의한 **사용자 정의 어트리뷰트**와 자바스크립트 간에 데이터를 교환할 수 있다.
data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 쓴다.

```html
<!DOCTYPE html>
<html>
<body>
  <ul class="users">
    <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
  </ul>
</body>
</html>
```

`HTMLElement.dataset` 프로퍼티로 data 어트리뷰트 값을 취득할 수 있다.
dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다.

DOMStringMap 객체 : data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가진다.
이 프로퍼티로 data 어트리뷰트 값을 취득하거나 변경할 수 있다.

data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.
이때 data 프로퍼티에 추가한 카멜케이스 프로퍼티 키는 data 어트리뷰트의 data- 접두사 다음에 케밥케이스로 자동 변경되어 추가된다.

```html
<!DOCTYPE html>
<html>
<body>
  <ul class="users">
    <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    <li id="2" data-user-id="9524">Kim</li>
  </ul>
  <script>
    const users = [...document.querySelector('.users').children];

    const user = users.find(user => user.dataset.userId === '7621');
    console.log(user.dataset.role); // "admin"

    user.dataset.role = 'subscriber';
    // dataset 프로퍼티는 DOMStringMap 객체를 반환한다.
    console.log(user.dataset); // DOMStringMap {userId: "7621", role: "subscriber"}
  </script>
</body>
</html>
```

## 스타일

### 인라인 스타일 조작

`HTMLElement.prototype.style` 프로퍼티는 요소 노드의 인라인 스타일을 취득하거나 추가/변경한다.

style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다.
CSSStyleDeclaration 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 갖고 있다.
이 프로퍼티에 값을 할당하면 해당 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.
CSS 프로퍼티는 케밥 케이스를 따른다. 대응하는 CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스를 따른다.

```html
<!DOCTYPE html>
<html>
<body>
  <div style="color: red">Hello World</div>
  <script>
    const $div = document.querySelector('div');

    // 인라인 스타일 취득
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }

    // 인라인 스타일 변경
    $div.style.color = 'blue';

    // 인라인 스타일 추가
    $div.style.width = '100px'; // 단위를 반드시 포함해야 한다.
    $div.style.height = '100px';
    $div.style.backgroundColor = 'yellow';
  </script>
</body>
</html>
```

### 클래스 조작

.으로 시작하는 클래스 선택자로 HTML 요소의 class 어트리뷰트 값을 변경해 스타일을 변경할 수 있다.

> class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아니라 className, classList다.
class는 예약어이기 때문!

#### className
Element.prototype.className 프로퍼티 : class 어트리뷰트 값을 취득하거나 변경
className은 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기 불편하다.

### classList
Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .box {
      width: 100px; height: 100px;
      background-color: antiquewhite;
    }
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<body>
  <div class="box red">Hello World</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
    // classList가 반환하는 DOMTokenList 객체는 HTMLCollection과 NodeList와 같이
    // 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) 객체다.
    console.log($box.classList);
    // DOMTokenList(2) [length: 2, value: "box blue", 0: "box", 1: "blue"]

    // .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
    $box.classList.replace('red', 'blue');
  </script>
</body>
</html>
```

> DOMTokenList 객체 : class 어트리뷰트 정보를 나타내는 컬렉션 객체. 유사 배열 객체이면서 이터러블

- add(...className) : class 어트리뷰트 값 추가
- remove(...className) : class 어트리뷰트 삭제
- item(index) : index에 해당하는 클래스 반환
- contains(className) : class 어트리뷰트에 포함되어 있는지 확인
- replace(oldClassName, newClassname) : oldClassName을 newClassName으로 변경
- toggle(classname[, force]) : 클래스가 존재하면 제거, 없으면 추가. 두번째 인수 불리언 값이 true면 강제로 문자열을 추가, flase면 강제로 제거한다.
- forEach, entries, keys, values, supports 메서드 제공

### 요소에 적용되어 있는 CSS 스타일 참조

style 프로퍼티는 인라인 스타일만 반환한다.
암묵적으로 적용된 스타일을 참조할 수 없는데, 이 때는 getComputedStyle을 사용해야 한다.

window.getComputedStyle(element,[, pseudo]) : element로 전달한 요소 노드에 적용된 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환. 두 번째 인수로는 :after, :before 등을 전달할 수 있다.
computed style : 모든 스타일(링크 스타일, 임베딩 스타일, 인라인 스타일, 자바스크립트에서 적용한 스타일, 상속된 스타일, 기본 스타일 등)

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      color: red;
    }
    .box {
      width: 100px;
      height: 50px;
      background-color: cornsilk;
      border: 1px solid black;
    }
    .box:before {
      content: 'Hello';
    }
  </style>
</head>
<body>
  <div class="box">Box</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
    const computedStyle = window.getComputedStyle($box);
    console.log(computedStyle); // CSSStyleDeclaration

    // 임베딩 스타일
    console.log(computedStyle.width); // 100px
    console.log(computedStyle.height); // 50px
    console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
    console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

    // 상속 스타일(body -> .box)
    console.log(computedStyle.color); // rgb(255, 0, 0)

    // 기본 스타일
    console.log(computedStyle.display); // block
    
    // 의사 요소 :before의 스타일을 취득한다.
    const pseudo = window.getComputedStyle($box, ':before');
    console.log(pseudo.content); // "Hello"
  </script>
</body>
</html>
```

## DOM 표준

HTML과 DOM 표준은 W3C과 WHATWG가 공통된 표준을 만들어 왔는데,
2018년 4월부터는 WHATWG(구글, 애플, 마이크로소프트, 모질라)가 단일 표준을 내놓기로 합의했다.

끝!!!!!