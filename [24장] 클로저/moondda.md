## 렉시컬 스코프
클로저는 **함수와 그 함수가 선언된 렉시컬 환경과의 조합**이다

```tsx
const x = 1;

function foo() {
	const x= 10;
	bar();
}

function bar(){
	console.log(x);
}

foo();
bar();
```
1,1이 출력된다 <br/>==
자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 `어디에 정의했는지`에 따라 `상위 스코프를 결정`하기 때문이다<br/>
함수의 호출 위치와 상위 스코프는 아무런 관계가 없다. <br/>
이를 **렉시컬 스코프**라고 한다.


## 클로저와 렉시컬 환경
```tsx
const x = 1;
function outer() {
  const x = 10;
  const inner = function () { console.log(x); }
  return inner;
}

const innerFunc = outer();
innerFunc(); //10
```
outer함수는 innerFunc 에 inner를 반환하고 생명주기가 끝난다. <br/>
그런데도 10(지역변수)이 잘 동작함<br/>
왜냐하면 outer함수의 실행 컨텍스트는 실행 컨테스트 스택에서 제거되지만 outer 함수의 `렉시컬 환경`(주변 스코프에 대한 정보를 관리하는 구조)까지 소멸하는 것은 아니기 때문.
<br/>

이처럼 외부함수보다 중첩 함수가 더 오래 유지되는 경우, 중첩 함수는 이미 생명 주기가 종료한 외부함수의 변수를 참조할 수 있다.<br/>
이러한 중첩 함수를 **클로저**라고 부른다.

## 클로저의 활용
상태를 의도치 않게 변경되지 않도록, 상태를 안전하게 은닉하고 특정함수에게만 상태 변경을 허용하기 위해 사용한다.

```tsx
let num = 0 ;
const increase = function() {
	return ++num;
};
console.log(increase()); //1
console.log(increase()); //2
console.log(increase()); //3
```
 위의 코드는 동작하는데 문제는 없지만 오류를 발생시킬 여지가 있다. num이 멋대로 값이 변경되면 안되기 때문. <br/><br/>
 클로저를 활용하여 바꿔보자 <br/>

 ```tsx
const increase = function() {
	let num = 0 ;
	return function() {
		return ++num;
	}
};
console.log(increase()); //1
console.log(increase()); //2
console.log(increase()); //3
```
																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																						       
