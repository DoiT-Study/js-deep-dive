# 15장 let, Const 키워드와 블록 레벨 스코프
## 15.1 var 키워드로 선언한 변수의 문제점

var 키워드는 변수를 선언하는데 사용된다.

- 또한, 이 키워드는 중복 선언이 가능하다.

```javascript

var x = 1;
var x = 32;
con sole.log(x); //x = 100

```
즉, 변수를 중복 선언할 때, 초기화문이 없는 변수를 무시하는 것과 같다.

- 이외에도, 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다.

이때, 함수 외부에서 선언한 변수는 모두 전역 변수이다.

- var 키워드로 선언된 변수는 변수 호이스팅에 의해 선언문이 선두로 끌어 올려진 것처럼 동작한다.

  그러나, 가독성과 같은 문제들이 발생할 수 있으므로 많이 사용되지는 않는 특징이다.

  ```javascript
  console.log(f); // undefined
  f = 2;
  var f; // var 선언문은 암묵적으로 먼저 실행됨
   console.log(f); // 2
   ```

## 15.2 let 키워드

- let 키워드는 var 키워드와 비교하자면 변수 중복 선언이 불가능하다.

```javascript
let e = 3;
let e = 13; // SyntexError
```

- 또한, var 키워드는 함수 레벨 스코프를 따르지만 let 키워드는 블록 레벨 스코프를 따른다.

  함수 레벨 스코프 : 함수의 코드 블록만을 지역 스코프로 인정

  블록 레벨 스코프 : 함수, if문, for문 등으로 이루어진 코드 블록을 지역 스코프로 인정

  ```javascript
  var ep = 3;
  {
   let ep = 32;
   let dj = 9;
  }
  console.log(ep);//3
  console.log(dj);//Error
  ```
  위 코드에서 console.log는 코드 블록 밖에 있으므로 전역 변수만을 취급한다.

   따라서 코드 블록 밖에 있는 ep변수 값인 3인 출력된다.

  그리고 dj변수는 코드 블록 안에 있으므로 지역 변수이다.

  따라서, console.log가 코드 블록 안에 있다면 9라는 값이 출력될 것이다.

 - 또한, var 키워드와 다르게 let 키워드는 변수 호이스팅이 발생하지 않는 것처럼 보인다.

   var 키워드에서 변수 호이스팅은 선언 단계와 초기화 단계가 합쳐져서 실행된다.
 
 그러나, let 키워드는 함수의 선언과 초기화 단계가 분리되어 진행되기 때문에 아래의 코드처럼 선언이 암묵적으로 실행되어도 초기화 되지 않은 상태로 접근한다면 에러가 발생하므로 변수 호이스팅이 지원되지 않는 것으로 보인다..

  ```javascript
   console.log(foo); // 참조 에러
   let foo;
  console.log(foo); //Undefined
  foo = 23;
   console.log(foo); //23
 ```

```javascript
let foo = 32; // 전역변수
{
console.log(foo); //ReferenceError
let foo = 3; // 지역변수
}
```
위의 코드에서 변수 호이스팅이 일어나지 않는다면 전역 변수 값인 32를 출력해야된다. 그러나 결론적으로 변수 호이스팅이 발생하므로 참조 에러가 발생한다.

- let 키워드로 선언한 전역 변수는 브라우저 환경에서 전역 개체 Window의 프로퍼티가 아니다.

  ```javascript
  let x = 3;
  console.log(window.x); //undefined
  console.log(x); // 3
  ```

  var 키워드는 선언단계를 거치지 않고 초기화 단계만을 거친 전역 변수를 window.x처럼 프로퍼티로 사용할 수 있다. 

 ## 15.3 Const 키워드

 const 키워드는 변수가 아닌 상수를 선언할 때 사용된다.

 - const 키워드를 사용해 변수를 선언할 때 무조건 선언과 초기화가 동시에 이뤄져야한다.

- let 키워드와 같이 블록 레벨 스코프 방식을 가져 변수 호이스팅이 일어나지 않는 것처럼 작동한다.

- var, let키워드와 다르게 const 키워드로 선언된 변수는 재할당이 금지된다.
  
재할당이 금지되기 때문에, const 키워드를 상수 표현으로 사용하는 것이다.    

```javascript
const Tax = 0.1; // 세율
let pretax = 100; // 세전
let aftertax = pretex + (pretex * Tax); // 세후
console.log(aftertax); //  110
```

위의 코드의 경우, Tax는 세율을 의미하므로 고정된 값임을 나타내기 위해 const 키워드를 사용했다.

또한, 세율이 변경하면 상수 값만 변경하면 되기 때문에 유지보수성이 좋다는 장점이 있다.

상수 변수를 선언할 때는 대문자로 시작하는 것이 좋고 여러 단어일 경우 _(underscore)를 넣는다.

- const 키워드로 선언된 변수에 객체를 할당할 경우, 객체는 변경이 가능하다.

  ```javascript
  const person = {
  name : 'shin' // name이라는 객체 생성
  };
  person.name = Lee; // 객체 값을 변경
  console.log(person); // Lee
  ```

## 15.4 var vs let vs const

const 키워드는 의도치 않은 재할당을 금지하기 때문에 var, let에 비해 안전하다

따라서, 일반적으로는 const 키워드를 사용하고 재할당이 필요하다면 let 키워드를 사용한다.

또한, ES6 버전의 경우에는 var 키워드를 거의 사용하지 않는다.

