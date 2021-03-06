# 23. 실행 컨텍스트

## 23.1 소스코드의 타입

ECMAScript 사양은 소스코드를 4가지 타입으로 구분하며, 4가지 타입의 소스코드는 실행 컨텍스트를 생성함

| 소스코드의 타입           | 설명                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 전역 코드 (global code)   | 전역에 존재하는 소스코드. 전역에 정의된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| 함수 코드 (function code) | 함수 내부에 존재하는 소스코드. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| eval 코드                 | 빌트인 전역 함수는 eval 함수에 인수로 전달되어 실행되는 소스코드 |
| 모듈 코드 (module code)   | 모듈 내부에 존재하는 소스 코드. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다. |

소스코드를 4가지 타입으로 구분하는 이유는 소스코드의 타입에 따라 실행 컨텍스트를 생성하는 과정과 관리 내용이 다르기 때문

##### 1. 전역코드

전역 코드는 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성. var 키워드로 생성된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고 참조하기 위해 전역 객체와 연결되어야 함. 이를 위해 전역 코드가 평가되면 전역 실행 컨텍스트가 생성됨

##### 2. 함수 코드

함수 코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리해야 하며, 생성된 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 함. 이를 위해 함수 코드가 실행되면 함수 실행 컨텍스트가 생성됨

##### 3. eval 코드

eval 코드는 strict mode 에서 자신만의 독자적인 스코프를 생성. 이를 위해 eval 코드가 평가되면 eval 실행 컨텍스트가 생성됨

##### 4. 모듈 코드

모듈 코드는 모듈별로 독립적인 모듈 스코프를 생성한다. 이를 위해 모듈 코드가 평가되면 모듈 실행 컨텍스트가 생성됨



## 23.2 소스코드의 평가와 실행

자바스크립트 엔진은 소스코드를 2개의 과정, **'소스코드의 평가'** 와 **'소스코드의 실행'** 과정으로 나누어 처리

**소스코드 평가과정 **

> 실행 컨텍스트 생성, 변수, 함수 등의 선언문 실행, 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록

**소스코드의 실행(런타임)**

> 소스코드 실행에 필요한 정보, 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득. 변수 값의 변경 등 소스코드 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 다시 등록

<img src="https://eunhyejung.github.io/assets/contents/js/content01_executioncontext.PNG"/>



## 23.3 실행 컨텍스트의 역할

#### 실행 컨텍스트

> 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역.
>
> 식별자(변수, 함수, 클래스 등의 이름)을 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리됨

식별자와 스코프틑 실행 컨텍스트의 렉시컬 환경으로 관리하고, 코드 실행 순서는 실행 컨텍스트 스택으로 관리

##### 1. 전역 코드 평가

전역 코드의 변수 선언문과 함수 선언문이 먼저 실행되고, 그 결과 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록됨.

이때 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 객체의 프로퍼티와 메서드가 된다.

##### 2. 전역 코드 실행

전역 코드 평가 과정이 끝나면 런타임이 시작되어 전역 코드가 순차적으로 실행. 이 때 전역 변수에 값이 할당되고 함수가 호출됨.

함수가 호출되면 순차적으로 실행되던 전역 코드의 실행을 일시 중단하고 코드 실행 순서를 변경하여 함수 내부로 진입한다.

##### 3. 함수 코드 평가

함수 코드 평가 과정을 거치며 함수 코드를 실행하기 위한 준비. 

매개변수와 지역 변수 선언문이 먼저 실행되고, 그 결과 생성된 매개변수와 지역 변수가 실행 컨텍스트가 관리하는 지역 스코프에 등록됨. 

또한 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체가 생성되어 지역 스코프에 등록되고,  this 바인딩도 결정된다.

##### 4. 함수 코드 실행

함수 코드 평가 과정이 끝나면 런타임이 시작되어 함수 코드가 순차적으로 실행. 이 때 매개변수와 지역 변수에 값이 할당되고 메서드가 호출됨.

console.log 메서드를 호출하기 위해 식별자인 console 을 스코프 체인을 통해 검색. 이를 위해 함수 코드의 지역 스코프는 상위 스코프인 전역 스코프와 연결되어야 함.

console 식별자는 스코프 체인에 등록되어 있지 않고 전역 객체의 프로퍼티로 존재하기 때문에 전역 스코프를 통해 검색이 가능해야 함.

log 프로퍼티를 console 객체의 프로토타입 체인을 통해 검색. 그후 console.log 메서드에 인수로 전달된 표현식 a + x + y 평가. a, x, y 식별자는 스코프 체인을 통해 검색

console.log 메서드의 실행이 종료되면 함수 코드 실행 과정이 종료되고 함수 호출 이전으로 돌아가 전역 코드 실행을 계속함

코드가 실행되려면 스코프, 식별자, 코드 실행 순서 등의 관리가 필요함

> 1. 선언에 의해 생성된 모든 식별자(변수, 함수, 클래스 등)를 스코프를 구분하여 등록하고, 상태 변화(식별자에 바인딩된 값의 변화)를 지속적으로 관리할 수 있어야 함
> 2. 스코프는 중첩 관계에 의해 스코프 체인을 형성해야 함. 스코프 체인을 통해 상위 스코프로 이동하며 식별자를 검색할 수 있어야함
> 3. 현재 실행 중인 코드의 실행 순서를 변경(ex. 함수 호출에 의한 실행 순서 변경)할 수 있어야 하며 다시 되돌아갈 수도 있어야 함



## 23.4 실행 컨텍스트 스택

실행 컨텍스트 스택 : 생성된 실행 컨텍스트는 스택(stack) 자료구조로 관리. 실행 컨텍스트 스택은 코드의 실행 순서를 관리

```js
const x = 1;

function foo(){
  const y = 2;
  
  function bar(){
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

위 코드를 실행하면 코드가 실행되는 시간의 흐름에 따라 실행 컨텍스트 스택에는 실행 컨텍스트가 추가(push) 되고 제거(pop)

<img src="https://jong-hui.github.io/assets/img/posts/execution-context/1.png" />

1. 전역 코드의 평가와 실행
2. foo 함수 코드의 평가와 실행
3. bar 함수 코드의 평가와 실행
4. foo 함수 코드로 복귀
5. 전역 코드로 복귀



## 23.5 렉시컬 환경

**렉시컬 환경 (Lexical Environment) **

> 식별자와 식별자에 바인딩된 값, 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트
>
> 키와 값을 갖는 객체 형태의 스코프(전역, 함수, 블록 스코프) 생성하여 식별자를 키로 등록하고 식별자에 바인딩된 값을 관리. 
>
> 즉, 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체

<img src="https://borachoi.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F052e4399-5782-4bba-8825-5d84cf2dabe9%2FUntitled.png?table=block&id=80a4f50b-432d-4daa-9d7e-d14091807a74&spaceId=2734092e-8132-4e66-9239-0ceddcedf0eb&width=1150&userId=&cache=v2" align="left" />

실행 컨텍스트는 `LexicalEnvironment` 컴포넌트와 `VariableEnvironment` 컴포넌트로 구성되며, 생성 초기의 각 컴포넌트는 동일한 렉시컬 환경을 참조한다.

이 후 몇가지 상황을 만나면 `VariableEnvironment` 컴포넌트를 위한 새로운 렉시컬 환경을 생성하고, 이때부터 두 컴포넌트의 내용이 달라지는 경우가 있음

<img src="https://borachoi.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4b9f75eb-ab6a-477d-856b-04777bfabf71%2FUntitled.png?table=block&id=cd25db1b-9863-4d7b-9cd9-5e7efea15b0f&spaceId=2734092e-8132-4e66-9239-0ceddcedf0eb&width=2000&userId=&cache=v2" align="left"/>

렉시컬 환경은 다음 두개의 컴포넌트로 구성된다.

1. 환경 레코드 (Environment Record)
	  * 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소

2. 외부 렉시컬 환경에 대한 참조 (Outer Lexical Environment Reference)
	  * 외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킴. 즉, 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경



## 23.6 실행 컨텍스트의 생성과 식별자 검색 과정

```js
var x = 1;
const y = 2;

function foo(a){
	var x = 3;
	const y = 4;
	
	function bar (b){
		const z = 5;
		console.log(a + b + x + y + z);
	}
	bar(10);
}

foo(20); //42
```

### 23.6.1 전역 객체 생성

​	전역 객체는 전역 코드가 평가되기 이전에 생성. 이때 전역 객체에는 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체가 추가되며,

​	동작 환경에 따라 클라이언트 사이드 Web api 또는 특정 환경을 위한 호스트 객체를 포함. 전역객체도 Object.prototype을 상속 받는다.

### 23.6.2 전역 코드 평가

#### 	1. 전역 실행 컨텍스트 생성

​		- 비어 있는 실행 컨텍스트를 생성하여 실행 컨텍스트 스택에 푸시

#### 	2. 전역 렉시컬 환경 생성

​		- 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩

##### 		2.1 전역 환경 레코드 생성

​			* 전역 렉시컬 환경을 구성하는 컴포넌트인 전역 환경 레코드는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체 제공

###### 			2.1.1 객체 환경 레코드 생성

​				var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수는 

​				전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 됨

###### 			2.1.2 선언전 환경 레코드 생성

​				let, const 키워드로 선언된 전역 변수는 선언적 환경 레코드에 등록되고 관리됨. 따라서 전역 객체의 프로퍼티가 되지 않기 때문에 window.y 와 같이 참조할 수 없음

​				선언 단계와 초기화 단계가 분리되어 진행되기 때문에, 런타임에 실행 흐름이 변수 선언문에 도달하기 전까지 일시적 사각지대 (Temporal Dead Zone; TDZ) 에 빠지게 됨

##### 		2.2 this 바인딩

​			* 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 this 가 바인딩.

##### 		2.3 외부 렉시컬 환경에 대한 참조 결정

​			* 외부 렉시컬 환경에 대한 참조는 현재 평가 중인 소스코드를 **포함**하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킴 

### 23.6.3 전역 코드 실행

### 23.6.4 foo 함수 코드 평가

#### 	1. 함수 실행 컨텍스트 생성

​		먼저 foo 함수 실행 컨텍스트를 생성 후, 함수 렉시컬 환경이 완성된 다음 실행 컨텍스트 스택에 푸시

#### 	2. 함수 렉시컬 환경 생성

​		foo 함수 렉시컬 환경을 생성하고 foo 함수 실행 컨텍스트에 바인딩

##### 		2.1 함수 환경 레코드 생성

​			매개변수, arguments 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수를 등록하고 관리

##### 		2.2 this 바인딩

​			함수 환경 레코드의 [[ThisValue]] 내부 슬롯에 this 바인딩

##### 		2.3 외부 렉시컬 환경에 대한 참조 결정

​			foo 함수 정의가 평가된 시점에 실행 중인 실행 컨텍스트(전역 코드)의 렉시컬 환경 참조가 할당

​			함수를 어디서 호출했는지가 아니라 어디에 정의했는지에 따라 상위 스코프 결정

### 23.6.5 foo 함수 코드 실행

### 23.6.6 bar 함수 코드 평가

​	bar 함수가 호출되면 bar 함수 내부로 코드의 제어권 이동

### 23.6.7 bar 함수 코드 실행

#### 	1. console 식별자 검색

#### 	2. log 메서드 검색

#### 	3. 표현식 a + b + x + y + z 의 평가

#### 	4. console.log 메서드 호출

### 23.6.8 bar 함수 코드 실행 종료

### 23.6.9 foo 함수 코드 실행 종료

### 23.6.10 전역 코드 실행 종료



## 23.7 실행 컨텍스트와 블록 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정하는 함수 레벨 스코프를 따르나,

let, const 키워드로 선언한 변수는 모든 코드 불록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따름	

```js
let x = 1;
if(true) {
	let x =10;
	console.log(x); //10
}

console.log(x); //1
```

if 문의 코드 블록이 실행되면 새로운 렉시컬 환경을 생성하여 기존 렉시컬 환경을 교체한다.

if문의 코드 블록이 종료 되면 이전 렉시컬 환경으로 복귀한다.



# 13. 스코프

## 13.1 스코프란?

**스코프 (Scope)** : 식별자가 유효한 범위

```js
var x = 'global';

function foo(){
  var x = 'local';
  console.log(x); // 1️⃣
}

foo();

console.log(x); // 2️⃣
```

1, 2 에서 x 변수를 참조할 때, 자바스크립트 엔진은 이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정해야 함.

**식별자 결정 (identifier resolution)** : 어느 스코프의 식별자를 참조하면 되는지 결정하는 것

식별자는 유일해야 하며, 스코프(유효범위)를 통해 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 함. 즉, 스코프는 네임스페이스



## 13.2 스코프의 종류

코드는 전역 (global), 지역 (local) 로 구분할 수 있다.

| 구분 | 설명                  | 스코프      | 변수      |
| ---- | --------------------- | ----------- | --------- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부        | 지역 스코프 | 지역 변수 |

### 

### 13.2.1 전역과 전역 스코프

<img src="https://velog.velcdn.com/images%2Fhang_kem_0531%2Fpost%2F338bea29-dac4-423a-a24f-3ea64ccf2d17%2Fimage.png"/>

전역이란 코드의 가장 바깥 영역을 말하며, 전역은 전역 스코프 (global scope) 를 만든다.

전역에 변수를 선언하면 전역 스코프를 갖는 **전역 변수 (global variable)** 이 된다. **전역 변수는 어디서든지 참조할 수 있다.**



### 13.2.2 지역과 지역 스코프

지역이란 함수 몸체 내부를 말하며, 지역은 지역 스코프 (local scope)를 만든다.

지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수 (local variable) 이 된다. 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.



## 13.3 스코프 체인

함수의 중첩 : 함수 몸체 내부에서 함수가 정의된 것.

중첩 함수 (nested function) : 함수 몸체 내부에서 정의한 함수

외부 함수 (outer function) : 중첩 하수를 포함하는 함수

함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있고, 이는 스코프가 함수의 중첩에 의해 계층적인 구조를 갖는다는 것을 의미함.

중첩 함수의 지역 스코프는 중첩 함수를 포함하는 외부 함수의 지역 스코프와 계층적 구조를 가지며, 이때 외부 함수의 지역 스코프를 중첩 함수의 상위 스코프라 함

모든 스코프는 하나의 계층적 구조로 연결되며, 모든 지역 스코프의 최상위 스코프는 전역 스코프

**스코프 체인 (scope chain)** : 스코프가 계층적으로 연결된 것

변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프부터 상위 스코프로 이동하며 선언된 변수를 검색. 

상위 스코프에서 선언된 변수를 하위 스코프에서도 참조할 수 있는 원리

상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만, 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수는 없다.



## 13.4 함수 레벨 스코프

지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만든다. 이는 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성된다는 의미

var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체) 만을 지역 스코프로 인정하는데, 이러한 특성을 함수 레벨 스코프라 함

if, for 문에서 선언된 var 키워드는 전역 변수임



## 13.5 렉시컬 스코프

동적 스코프 (dynamic scope) : 함수를 어디서 호출했는지에 따라 함수의 상위 스코프 결정

렉시컬 스코프 (lexical scope =  정적스코프; static scope) : 함수를 어디서 정의했는지에 따라 함수의 상위 스코프 결정

자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 정의했는지에 따라 상위 스코프를 결정함.
