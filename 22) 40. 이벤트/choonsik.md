# 이벤트

## 이벤트 드리븐 프로그래밍

이벤트 핸들러 : 이벤트가 발생했을 때 호출될 함수
이벤트 핸들러 등록 : 이벤트가 발생했을 때 이벤트 핸들러의 호출을 위임하는 것
이벤트 드리븐 프로그래밍 : 프로그램 흐름을 이벤트 중심으로 제허하는 프로그래밍 방식

## 이벤트 타입

### 마우스 이벤트

click, dbclick, mousedown, mouseup, mousemove, mouseenter, mouseover, mouseleave, mouseout

### 키보드 이벤트

keydown, keypress, keyup

### 포커스 이벤트

focus, blur, focusin, focusout

### 폼 이벤트

submit, reset

### 값 변경 이벤트

input, change, readystatechange

### DOM 뮤테이션 이벤트

DOMContentLoaded

### 뷰 이벤트

resize, scroll

### 리소스 이벤트

load, unload, abort, error

## 이벤트 핸들러 등록

### 이벤트 핸들러 어트리뷰트 방식

이벤트 어트리뷰트의 이름 : on접두사 + 이벤트 타입
함수 참조가 아닌 함수 호출문 등의 문을 할당한다.

이벤트 핸들러 어트리뷰트 값은 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다.

### 이벤트 핸들러 프로퍼티 방식

이벤트타깃.이벤트타임 = 이벤트 핸들러

### addEventListener 메서드 방식

이벤트타깃.addEventListener(이벤트타입, 이벤트 핸들러[, capture 사용 여부])

## 이벤트 핸들러 제거

이벤트타깃.removeEventListener(이벤트타입, 이벤트 핸들러([, capture 사용 여부]) // 인수가 일치해야 한다.

## 이벤트 객체

동적으로 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.

### 이벤트 객체의 상속 구조

이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있다.

### 이벤트 객체의 공통 프로퍼티

type, target, currentTarget, eventPhase, bubbles. cancelable, defaultPrevented, isTrusted, timeStamp

### 마우스 정보 취득

마우스 포인터의 좌표 정보 프로퍼티 : screenX/Y, clientX/Y, pageX/Y, offsetX/Y
버튼 정보 프로퍼티 : altKey, ctrlKey, shiftKey, button

### 키보드 정보 취득

altKey, ctrlKey, shiftKey, matakey, key, keyCode

## 이벤트 전파

DOM 트리를 통해 이벤트 타깃을 중심으로 이벤트가 전파되는 것

캡처링 단계 - 타깃 단계 - 버블링 단계

버블링을 통해 전파되지 않는 이벤트
포커스 이벤트 : focus/blur
리소스 이벤트 : load/unload/abort/error
마우스 이벤트 : mouseenter/mouseleave

## 이벤트 위임

하나의 상위 DOM 요소에 이벤트 핸들러를 등록한다.
이벤트 타깃을 검사해야한다.

Element.prototype.matches

## DOM 요소의 기본 동작 조작

### DOM 요소의 기본 동작 중단

preventDefault

### 이벤트 전파 방지

stopPropagation

## 이벤트 핸들러 내부의 this

### 이벤트 핸들러 어트리뷰트 방식

일반 함수로서 호출 되는 함수 내부의 this는 전역 객체
이벤트 핸들러 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소

### 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

this는 이벤트를 바인딩한 DOM 요소. 이벤트 객체의 currentTarget 프로퍼티
화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this

클래스에서 이벤트 핸들러를 바인딩하는 경우 this는 이벤트를 바인딩한 DOM 요소. bind를 사용해 클래스가 생성할 인스턴스를 가리키도록 해야 한다. 혹은 화살표 함수를 이벤트 핸들러로 등록하기

## 이벤트 핸들러에 인수 전달

이벤트 핸들러 어트리뷰트 방식 : 함수 호출문 사용 가능. 인수 전달 가능
이벤트 핸들러 프로퍼티 방식, addEventListener : 함수 자체를 등록. 인수 전달 불가능. 이벤트 핸들러 내부에서 함수를 호출해 인수를 전달해야 하거나 이벤트 핸들러를 반환하는 함수를 호출하며 인수를 전달해야 한다.

## 커스텀 이벤트

### 커스텀 이벤트 생성

Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출해 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.

첫번째 인자로 이벤트 타입
preventDefault 로 취소할 수 없고 bubbles, cancelable 프로퍼티 값이 false로 설정된다.

두번째 인자로 프로퍼티를 전달한다. (detail 프로퍼티를 포함하는 객체 전달)

### 커스텀 이벤트 디스패치

dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)

이벤트 핸들러를 동기처리 방식으로 호출한다.

