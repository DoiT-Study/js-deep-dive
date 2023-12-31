## 13.1 스코프란?

스코프 
* 식별자(변수 이름, 함수 이름, 클래스 이름 등)가 유효한 범위
* 식별자는 유일해야 한다. 다른 스코프에서는 같은 이름의 식별자 사용 가능 <br>
스코프는 식별자를 구분해주는 역할 함 => 네임스페이스라고 불림 

식별자 결정: 자바스크립트 엔진은 이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정하는 것
=>자바스크립트 엔진은 코드를 실행할 때 코드의 문맥, 즉 렉시컬 환경을 고려하여 식별자 결정을 한다.

` 렉시컬 환경: 코드가 어디서 실행되며 주변에 어떤 코드가 있는지를 의미`

같은 이름의 식별자를 사용하는 예시 
```
var x = 'jino';

function name(){
  var x = 'jinho';
  console.log(x);
}

name();

console.log(x);

```

var과 let, const의 차이

* var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언이 허용된다. => 의도치 않게 변수값이 재활당되어 변경되는 부작용을 발생시킴

* let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다. 


위의 예시에서 var대신에 let을 사용하면 에러 뜸<br>
why: 스코프가 겹쳐 let은 에러 발생, var는 스코프가 겹쳐도 상관없기에 에러가 뜨지 않는다. 함수 내에서 x변수를 선언할 때 var 키워드가 없는 것처럼 동작한다.


## 13.2 스코프의 종류
### 전역과 전역스코프
* 전역: 코드의 가장 바깥 영역
* 전역 스코프는 코드의 모든 부분이 유효범위이다
* 전역변수: 전역에 선언된 변수이고 전역 스코프를 갖는다.
* 전역 변수는 어디서든지 참조할 수 있다.
* 함수 내부에서도 참조할 수 있다.

### 지역과 지역 스코프
* 지역: 함수 몸체 내부
* 지역 변수: 지역에서 선언된 변수 
* 지역 변수는 자신이 선언된 지역과 하위 지역(중첩함수)에서만 참조할 수 있다.

### 예시
```
var x = "global x";

function outer(){
  var y = 'outer's local y";

  function inner(){
    var z = "inner's local z";
  }
}
```
* x는 전역 변수이다.
* y는 함수 outer의 지역 변수이다. 
* 지역 변수 y의 스코프는 함수 outer 내부이다. 따라서 함수 inner내부도 스코프에 포함된다.
* z는 함수 inner의 지역 변수이다.
* 지역 변수 z의 스코프는 함수 inner 내부이다.
* y와 z를 전역에서 사용하면 에러 발생, z를 outer에서 사용하면 에러 발생

## 13.3 스코프 체인
함수 중첩으로 사용 -> 스코프 중첩된다 -> 스코프가 계층적 구조 지님

스코프 체인: 스코프가 계층적으로 연결된 것

변수 검색<br>
스코프 체인은 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 변수 검색<br>
=>상위 스코프에서 선언한 변수를 하위 스코프에서도 참조 가능


## 13.4 함수 레벨 스코프

블록 레벨 스코프: 함수 몸체만이 아니라 모든 코드 블록(if, while, for등)이 지역 스코프를 만든다. (let, const)

함수 레벨 스코프:  함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정(var)

var 키워드로 선언된 변수는 함수 레벨 스코프만 인정
```
var i = 10;

for(var i=0; i < 5; i++){
  console.log(i);
}
console.log(i); 

```
=> for 문의 i가 var로 선언되었기에 for문을 지역 스코프로 인정하지 않아 for 문의 i는 전역변수이다. 


## 13.5 렉시컬 스코프
동적 스코프:함수를 어디서 호출했느지에 따라 결정되는 스코프, 함수가 호출되는 시점이 동적으로 결정되기 때문에 동적 스코프라 한다.


렉시컬 스코프 or 정적 스코프: 함수 정의에 따라 결정되는 스코프, 함수 정의는 동적이지 않고 정적이므로 정적 스코프라 불림
