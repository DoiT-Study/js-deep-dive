# 47. 에러처리

# 47.1 에러 처리의 필요성

> 에러가 발생하지 않는 코드를 작성하는 것은 불가능하다. 이때 발생한 에러에 대해 어떻게 대처하느냐가 중요하다.

```js
console.log("[Start]");

foo();

console.log("[End]");
```

```
C:\Users\user\OneDrive\바탕 화면\js-deep-dive>node "c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js"
[Start]
c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:3
foo(); // ReferenceError: foo is not defined
```

```js
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector("button"); // null

$button.classList.add("disabled");
```

```
C:\Users\user\OneDrive\바탕 화면\js-deep-dive>node "c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js"
c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:2
const $button = document.querySelector("button"); // null
                ^
```

# 47.2 try...catch...finally문

- 에러 처리를 구현하는 방법

1.  예외적인 상황이 발생하면 반환하는 값(null or -1)을 if문이나 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
2.  try...catch...finally 문을 활용하여 처리하는 방법

```js
try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행된다.
}
```

```js
console.log("[Start]");

try {
  foo();
} catch (error) {
  console.log("에러 발생!");
} finally {
  console.log("finally");
}

console.log("[End]");
```

```
[Start]
에러 발생!
finally
[End]
```

# 47.3 Error 객체

> Error 생성자 함수는 에러 객체를 생성한다. Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인스턴스로 전달할 수 있다.

```js
const error = new Error("invalid");
```

Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.
message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이고, stack 프로퍼티의 값은 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용된다.

- Error 생성자 함수 : 일반적 에러 객체
- SyntaxError 생성자 함수 : JS 문법에 맞지 않는 문 해석할 때 발생하는 에러 객체
- ReferenceError 생성자 함수 : 참조할 수 없는 식별자 참조 시 발생하는 에러 객체
- TypeError 생성자 함수 : 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체
- RangeError 생성자 함수 : 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체
- URIError 생성자 함수 : encodeURI 또는 decodeURI 함수에 부적절한 인수 전달했을 때 발생하는 에러 객체
- EvalError 생성자 함수 : eval 함수에서 발생하는 에러 객체

# 47.4 throw문

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉, 에러 객체 생성과 에러 발생은 의미가 다르다.

```js
try {
  // 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
  new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다.

> throw 표현식

```js
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error("something wrong");
} catch (error) {
  console.log(error); // Error: something wrong
}
```

```
Error: something wrong
```

# 47.5 에러의 전파

> 에러는 호출자 방향으로 전파된다. 즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

```js
const foo = () => {
  throw Error("foo에서 발생한 에러"); // ④
};

const bar = () => {
  foo(); // ③
};

const baz = () => {
  bar(); // ②
};

try {
  baz(); // ①
} catch (err) {
  console.error(err);
}
```

```
Error: foo에서 발생한 에러
    at foo (c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:2:9)
    at bar (c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:6:3)
    at baz (c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:10:3)
    at Object.<anonymous> (c:\Users\user\OneDrive\바탕 화면\js-deep-dive\asd.js:14:3)
```

throw된 에러는 foo -> bar -> baz -> 전역 실행 컨텍스트 방향으로 전파된다.
throw된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.

> 주의: 비동기 함수는 에러를 전파할 caller가 존재하지 않음 ( 예: setTimeout, 프로미스 후속 처리 메서드의 콜백 함수)
