# DOM

> DOM은 HTML 문서의 계층적 구조와 정보를 포현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

## 노드

- HTML 요소 간에는 중첩 관계에 의해 계층적인 부자(엄마-아들) 관계가 형성된다. 이런 관계를 파악해서 트리 자료 구조로 구성한다.

### 노드의 구조는 12개의 종류

- 문서 노드: DOM 트리의 최상위에 존재하는 루트 노드
- 요소 노드: HTML 요소를 가리키는 객체
- 어트리뷰트 노드: HTML 요소의 어트리뷰트를 가리키는 객체
- 텍스트 노드: HTML 요소의 텍스트를 가리키는 객체다. 텍스트 노드는 DOM 트리의 최종단이다.
- 이 외에도 주석 노드를 위한 Comment, DOCTYPE을 위한 DocumentType노드, 여러 노드들이 존재한다.

## 노드 객체의 상속 구조

> HTML input을 살펴보면...

프로토타입 체인 관점에서 살펴봐서 input 요소를 파싱해서 객체화한 input 요소 노드 객체는 HTMLInputElement -> HTMLElement -> Element -> Node -> EventTarget -> Object로 상속받고 있다.

> DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 요소 노드 취득

### id를 이용한 요소 노드 취득

- id는 중복될 수 있다.
- id가 중복 되어도 에러가 나지 않는다.

### tag 이름을 이용한 요소 노드 획득

- getElementsByTagName 메서드가 반환하는 객체는 이터러블하고 유사 배열이다.

## 요소 노드의 텍스트 조작

> nodeValue, textContent, innerText

- nodeValue는 사용하려면 텍스트 노드에 접근해야해서 접근 방식이 조금 더 복잡하다.
- textContent는 HTML 마크업이 파싱되지 않는다.
- innerText는 CSS visibility 속성이 hidden이면 요소 노드의 텍스트를 반환하지 않는다.
- innerText는 CSS를 고려하므로 textContent보다 느리다.

## innerHTML의 위험성

- innerHTML은 XSS (cross-site-scripting), HTML 마크업 내에 자바스크립트 악성 코드가 포함되어 있으면 파싱 과정에서 그대로 실행될 가능성이 있다.
- DOMPurify.sanitize() 를 이용해서 잠재적 위험을 제거할 수 있긴하다.
