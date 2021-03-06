# 25. 클래스

# 25.1 클래스는 프로토타입의 문법적 설탕인가?

> 자바스크립트 프로토타입의 언어이기 때문에 객체 지향적인 클래스가 필요없었다.

클래스와 생성자 함수가 다른 점

1. 클래스를 new 연사자 없이 호출하면 에러가 발행한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
5. 클래스의 컨스트럭트, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 false다. 다시 말해, 열거되지 않는다.

# 25.2 클래스 정의

> 클래스는 class 키워드를 사용하여 정의한다.

클래스는 일급 객체로서 밑의 특징을 가진다.

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조에 저장할 수 있다.
3. 함수의 매개변수에게 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다

- constructor (생성자)
- 프로토타입 메서드
- 정적 메서드

# 25.3 클래스 호이스팅

> 클래스는 함수로 평가된다.

- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이 때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수 즉 constructor다.
- 클래스는 클래스 정의 이전에 참조할 수 없다.
- 클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅된다.
- 그래서 클래스도일시적 사각지대에 빠질 수 있다.

# 25.4 인스턴스 생성

> `new` 연산자와 함께 호출되어서 인스턴스를 생성할 수 있다.

- `new` 연산자를 사용하지않으면 타입 에러가 발생한다.

# 25.5 메서드

> 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 세 가지가 있다.

## 25.5.1 constructor

> constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. constructor는 이름을 변경할 수 없다.

- constructor안에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다.
- 생성자 === constructor

클래스가 생성한 인스턴스에는 생성자 메서드가 보이지 않는다. 이는 생성자 함수가 단순한 메서드가 아니란 뜻이다.
생성자는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.

생성자에는 몇 가지 특징이 있다.

- 생성자 메서드는 클래스 내 최대 한 개만 존재할 수 있다.
- 생성자 메서드는 생략할 수 있다.
- 인스턴스를 초기화하려면 생성자 메서드를 생략하면 안된다.
- 생성자 메서드는 반환문이 없다. 암묵적으로 this, 즉 인스턴스를 반환한다.
- 만약 return을 쓴다면 this 반환이 무시된다. (반드시 하면 안되는 행동)

## 25.5.2 프로토타입 메서드

> 클래스 몸체에 일반적으로 생성한 메서드는 프로토타입 메서드가 된다.

- 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.
- 인스턴스는 프로토타입 메서드를 상속받아서 사용할 수 있다.

## 25.5.3 정적 메서드 (몰랐음)

> 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드다. `static` 키워드를 붙이면 된다.

- 정적 메서드는 클래스에 바인딩된 메서드가 된다. 프로토타입 메서드는 프로토타입에 들어간다.
- 정적 메서드는 클래스 정의 이후에 인스턴스를 생성하지 않아도 호출할 수 있다.
- 정적 메서드는 인스턴스로 호출할 수 없다. 왜냐하면 프로토타입 체인상에 존재하지 않기 때문

## 25.5.4 정적 메서드와 프로토타입 메서드의 차이

1. 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출, 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입은 참조할 수 있다.

## 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약표현을 사용한다.
2. 암묵적으로 strict mode가 실행
3. for ... in 문이나 Object.keys와 같은 메서드 등으로 열거할 수 없다.
4. `new` 연산자와 함께 호출할 수 없다.

# 25.6 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩 - 인스턴스는 this에 바인딩된다.
2. 인스턴스 초기화 - this에 바인딩되어 있는 인스턴스를 초기화한다. 프로퍼티도 초기화 한다. 만약 생성자가 없으면 이 과정이 생략된다.
3. 인스턴스 반환 - 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

# 25.7 프로퍼티

## 25.7.1 인스턴스 프로퍼티

> 인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

- 기본적으로 Public 이다.
- constructor안에서 private, public, protected와 같은 키워드와 같은 접근 제한자를 지원하지 않는다.

## 25.7.2 접근자 프로퍼티

- getter, setter와 함수를 제공한다.
- setter는 딱 하나의 매개변수만 선언 가능하다.

## 25.7.3 클래스 필드 정의 제안

- 인스턴스를 생성할 때 클래스 필드를 초기화할 필요가 있다면 constructor 밖에서 클래스 필드를 정의할 필요가 없다.

## 25.7.4 private 필드 정의 제안

- 인스턴스 프로퍼티는 항상 Pulbic이기 때문에 private로 선언할 것이 필요했다.
- `#` 키워드를 필드 앞에 붙이면된다.

## 25.7.5 static 필드 정의 제안

- 프로퍼티에도 static을 붙일 수 있다.

# 25.8 상속에 의한 클래스 확장

# 25.8.1 클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.
- extends 키워드를 사용해서 상속하고 받을 수 있다.

# 25.8.2 extends 키워드

- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.
- 상속을 통해 확장된 클래스를 서브클래스, 서브클래스에게 상속된 클래스를 수퍼클래스라 부른다.
- 서브클래스를 자식클래스, 수퍼클래스를 부모클래스라고 부르기도 한다.

# 25.8.5 super 키워드

> super 키워드는 함수처럼 호출할 수도 있고, this와 같이 식별자처럼 참조할수 있는 특수한 키워드다

super의 동작방식은 아래와 같다.

- super를 호출하면 수퍼클래스(부모)의 constructor를 호출한다.
- super를 참조하면 수퍼클래스(부모)의 메서드를 호출할 수 있다.

만약에 상속을 받는 서브클래스는 자동적으로 부모클래스의 super를 실행시켜서 constructor가 실행이 되는 것이다.

super를 호출할 때 주의할 점은 다음과 같다.

- 자식클래스에서 constructor를 생략하지 않는 경우 자식클래스의 constructor에서는 반드시 super를 호출해야 한다.
- 자식클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
- super는 반드시 자식클래스의 constructor에서만 호출한다. 자식클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.

# 25.8.6 상속 클래스의 생성 과정

1. 자식 클래스의 super 호출
2. 부모 클래스의 인스턴스 생성과 this 바인딩
3. 부모 클래스의 인스턴스 초기화 (부모 클래스 constructor 실행)
4. 자식 클래스 constructor로의 복귀와 this 바인딩
5. 자식 클래스 인스턴스 초기화 (자식 클래스 constructor 실행)
6. 인스턴스 최종 반환
