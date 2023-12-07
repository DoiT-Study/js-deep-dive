에러가 발생하지 않는 코드를 작성하는 것은 불가능하다. 에러는 언제나 발생할 수 있다.

try ... catch 문을 사용해, 에러에 적절하게 대응하면, 프로그램이 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.

## **try ... catch ... finally 문**

예외 처리를 구현하는 방법은 크게 두 가지가 있다.

예외적인 상황이 발생하면 반환하는 값(null 또는 -1)을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법과, 에러 처리 코드를 미리 등록해 두고 에러가 발생하면, 에러 처리 코드로 점프하도록 하는 방법이 있다.

try ... catch ... finally 문은 두 번째 방법이다.

try ... catch ... finally 문을 실행하면 먼저 try 코드 블록이 실행된다. 이때, try 코드 블록에 포함된 문 중에서 에러가 발생하면, 발생한 에러는 catch 문의 매게변수로 전달되고 catch 코드 블록이 실행된다.

catch 문의 매개변수는 try 코드 블록에 포함된 문 중에서 에러가 발생하면 생성되고, catch 코드 블록에서만 유효하다.

finally 코드 블록은 에러 발생과 상관없이 반드시 한번 실행된다.

```jsx
console.log('[Start]');

try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  foo();
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
  console.error(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행된다.
  console.log('finally');
}

// try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
console.log('[End]');
```

# Error 객체

Error 생성자 함수는 에러 객체를 생성한다. 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다

생성된 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.

message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이고, stack 프로퍼티의 값은 에러를 발생시킨 콜 스택의 호출 정보를 나타낸다.

자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공한다. (SyntaxError, ReferenceError, TypeError, RangeError, URIError, EvalError)

# throw 문

에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 준다.

throw 문의 표현식은 어떤 값이라도 상관없지만, 일반적으로 에러 객체를 지정한다. 에러를 던지면 catch 문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다.

```jsx
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error('something wrong');
} catch (error) {
  console.log(error);
}
```

# 에러의 전파

에러는 호출자 방향 (즉, 콜 스택의 아래 방향) 으로 전파된다.
