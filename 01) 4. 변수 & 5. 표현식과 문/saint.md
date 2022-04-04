## 04. 변수

변수 (variable) : 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름

Mdn에서의 정의 : 값을 저장하기 위해 명명된 위치



메모리 공간에 저장된 값을 다시 읽어 재사용할 수 있도록 값이 저장된 메모리 공간에 상징적인 이름을 붙인 것이 바로 변수

<img src="/Users/seongtaek/project/TIL/딥다이브스터디/variable.png" align="left">



##### 변수가 왜 필요할까? 

연산 결과와 같은 특정한 값을 재사용하려면 값이 저장되어 있는 메모리에 접근해야 되는데 메모리 주소를 통해 값에 직접 접근하는 것보다 변수를 통해 안전하게 값에 접근하기 위함



변수명 (변수이름) : 메모리 공간에 저장된 값을 식별할 수 있는 고유한 이름

변수값 : 변수에 저장된 값

할당 (assignment / 대입, 저장) : 변수에 값을 저장하는 것

참조 (reference) : 변수에 저장된 값을 읽어 들이는 것



식별자(identifier) : 어떤 값을 구별해서 식별할 수 있는 고유한 이름. 값이 아니라 메모리 주소를 기억하고 있음. 다시 말해서 메모리 주소에 붙인 이름이라고도 할 수 있음

변수명뿐 아니라 변수, 함수, 클래스 등의 이름을 모두 식별자라고 함



#### 변수 선언 (variable declaration)

변수 선언 : 변수를 생성하는 것. 값을 저장하기 위한 메모리 공간을 확보하고 변수 이름과 확보된 메모리 공간의 주소를 연결하여 값을 저장할 수 있게 준비하는 것.

변수를 사용하기 위해서는 반드시 선언이 필요하며, 변수를 선언할 때는 var, let, const 키워드를 사용함

```js
var score;
```

위의 예시문에서는 변수를 선언하고 값을 할당하지 않았는데, 자바스크립트 엔진에 의해 확보된 메모리 공간에 undefined 라는 값을 할당하여 초기화

이를 initialization 이라고 하며 var 키워드에서만 해당. let, const 로 선언한 변수는 선언 전에 사용시 ReferenceError가 발생.



##### 자바스크립트 엔진이 변수 선언을 수행하는 과정

1. 선언 단계 : 변수 이름을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다.
2. 초기화 단계 : 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined 를 할당하여 초기화



변수를 사용하기 위해서는 반드시 변수 선언이 필요하며, 선언하지 않은 식별자에 접근하면 ReferenceError(참조 에러)가 발생.

<img src="/Users/seongtaek/project/TIL/딥다이브스터디/referenceError.png" align="left">



#### 변수 호이스팅

변수 선언은 런타임 시점에서 실행되는 것이 아니라 그 이전 단계에서 실행되게 됨.

따라서 변수 선언문은 코드의 선두로 끌어 올려진 것처럼 동작하는데 이를 변수 호이스팅이라 함.

```js
console.log(scroe); // undefined

var score;
```

위의 예시문에서 score 변수의 선언이 런타임보다 먼저 실행되기 때문에 ReferenceError를 발생하는게 아닌 undifined 를 출력



#### 할당 (assignment)

할당 연산자 = 를 사용하며, 우변의 값을 좌변의 변수에 할당

```js
var score; // 변수 선언
score = 80; // 값의 할당
```



변수 선언과 할당을 하나의 문으로 표현할 수도 있다.

```js
var score = 80;
```

위처럼 선언과 할당을 하나의 문으로 단축 표현해도 자바스크립트 엔진은 2개의 문으로 나누어 각각 실행하며,

변수 선언은 런타임 이전에 값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행



#### 값의 재할당

재할당 : 이미 값이 할당되어 있는 변수에 새로운 값을 또다시 할당하는 것

```js
var score = 80;
score = 90;
```

재할당을 하게 되면 score 변수의 값은 이전 값 80에서 재할당한 값 90으로 변경되는데 이 때 80이 저장되어있던 메모리 값을 지우고 90을 할당하는 것이 아니라,

새로운 메모리 공간을 확보하고 그 메모리 공간에 숫자 값 90을 할당한다.

<img src="/Users/seongtaek/project/TIL/딥다이브스터디/재할당.png" align=left>

이 때 이전 값인 80과 undefined 는 어떤 식별자와도 연결되지 않은 상태가 되고 이러한 불필요한 값들은 가비지 콜렉터에 의해 메모리에서 자동 해제됨.



##### 식별자 네이밍 규칙

- 특수문자를 제외한 문자, 숫자, 언더스코어(_), 달러기호($)를 포함할 수 있다.
- 숫자로 시작하는 것은 금지
- 예약어는 식별자로 사용 금지

| abstract | arguments  |    await*    |  boolean  |
| :------: | :--------: | :----------: | :-------: |
|  break   |    byte    |     case     |   catch   |
|   char   |   class*   |    const     | continue  |
| debugger |  default   |    delete    |    do     |
|  double  |    else    |    enum*     |   eval    |
| export*  |  extends*  |    false     |   final   |
| finally  |   float    |     for      | function  |
|   goto   |     if     |  implements  |  import*  |
|    in    | instanceof |     int      | interface |
|   let*   |    long    |    native    |    new    |
|   null   |  package   |   private    | protected |
|  public  |   return   |    short     |  static   |
|  super*  |   switch   | synchronized |   this    |
|  throw   |   throws   |  transient   |   true    |
|   try    |   typeof   |     var      |   void    |
| volatile |   while    |     with     |   yield   |

Words marked with* are new in ECMAScript 5 and 6.



##### 네이밍 컨벤션 : 가독성 좋게 단어를 구분하기 위해 규정한 명명 규칙

```js
// 카멜 케이스 (camelCase) : 중간 단어만 첫 글자를 대문자로
var firstName;

// 파스칼 케이스 (PascalCase) : 첫 단어와 중간 단어 모두 첫 글자를 대문자로
var FirstName;

// 스네이크 케이스 (snake_case) : 단어 사이를 언더바(_)로 연결
var first_name;

// 헝가리언 케이스(typeHungarianCase) : 변수의 타입을 같이 명시
var strFirstName; // type + identifier
var $elem = document.getElementById('myId'); // DOM node
var 
```



## 05. 표현식과 문

# 값

> 값(value)는 식(expression)이 평가(evaluate)되어 생성한 결과를 말한다.

변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름이다.

따라서 `변수에 할당`되는 것은 값이다.

# 리터럴

> 리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법(notation)을 말한다.

리터럴은 값을 생성하기 위해 미리 약속한 표기법이라고 할 수 있다.

| 리터럴             | 예시                              |
| ------------------ | --------------------------------- |
| 정수 리터럴        | 100                               |
| 문자열 리터럴      | 'Hello', "world                   |
| 불리언 리터럴      | true, false                       |
| null 리터럴        | null                              |
| undefined 리터럴   | undefined                         |
| 객체 리터럴        | {name : "Lee", address : 'Seoul'} |
| 배열 리터럴        | [1, 2, 3]                         |
| 함수 리터럴        | function() {}                     |
| 정규 표현식 리터럴 | /[A-Z]+/g                         |



# 표현식

> 표현식(expression)은 값으로 평가될 수 있는 문(statement)이다. 즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값ㅇ르 참조한다.

값으로 평가될 수 있는 문은 모두 표현식이다.

# 문

> 문(statement)은 프로그램을 구성하는 기본 단위이자 최소 실행 단위다.

![img](https://borachoi.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff298e2f2-b9c6-470f-a1bd-6f58736a5c99%2FUntitled.png?table=block&id=d1469c8f-7445-492b-aff6-797ea10261bd&spaceId=2734092e-8132-4e66-9239-0ceddcedf0eb&width=2000&userId=&cache=v2)

토큰(token)이란 문법적인 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미한다.

문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다.

```jsx
//변수 선언문
var x;

//할당문
x = 5;

//함수 선언문
function foo(){}

//조건문
if(x>1){console.log(x);}

//반복문
for(var i =0;i<2;i++) {console.log(i); }
```

# 세미콜론과 세미콜론 자동 삽입 기능

세미콜론(;)은 문의 종료를 나타낸다. 자바스크립트 엔진은 세미콜론으로 문이 종료한 위치를 파악하고 순차적으로 하나씩 문을 실행한다. 문을 끝낼 때는 세미콜론을 붙여야 한다. 단, 0개 이상의 문을 중괄호로 묶은 코드 블록 뒤에는 세미콜론을 붙이지 않는다. 이러한 코드 블록은 언제나 문의 종료를 의미하는 자체 종결성(self closing)을 갖기 때문이다.

세미콜론은 생략 가능하다. 자사브크립트 엔진이 세미콜론 자동 삽입 기능(ASI, automatic semicolon insertion)이 암묵적으로 수행되기 때문이다.

# 표현식인 문과 표현식이 아닌 문

표현식은 문의 일부일 수도 있고 그 자체로 문이 될 수도 있다.

```jsx
//변수 선언문은 값으로 평가될 수 없으므로 표현식이 아니다.
var x;
//1,2,1+2,x=1+2는 모두 표현식이다.
//x=1 + 2는 표현식이면서 완전한 문이기도 하다.
x=1 + 2;
```

표현식인 문은 값으로 평가될 수 있는 문이며, 표현식이 아닌 문은 값으로 평가될 수 없는 문을 말한다.

표현식인 문과 표현식이 아닌 문을 구별하는 가장 간단하고 명료한 방법은 변수에 할당해 보는 것이다.

표현식인 문은 값으로 평가되므로 변수에 할당할 수 있다. 하지만 표현식이 아닌 문은 값으로 평가할 수 없으므로 변수에 할당하면 에러가 발생한다

```jsx
//변수 선언문은 표현식이 아닌 문이다.
var x;

//할당문은 그 자체가 표현식이지만 완전한 문이기도 하다. 즉, 할당문은 표현식인 무니다.
x = 100;

//표현식이 아닌문은 값처럼 사용할 수 없다.
var foo = var x; //SyntaxError : Unexpected token var
```