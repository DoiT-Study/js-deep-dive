결론부터 박고 시작하겠습니다.
콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고, 그 수가 많아질수록 한번에 처리하는 데도 한계가 있다
이 콜백 패턴을 보완하기 위해 es6에서는 '프로미스;라는 또다른 비동기 처리를 위한 패턴이 등장하였다.

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

콜백 함수가 뭔지부터..<br>
`
A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.`

비동기 함수란

- 함수 내부에 비동기로 동작하는 코드를 **포함한** 함수
- 호출 시 내부의 비동기 코드가 완료되지 않아도, 기다리지 않고 종료해버린다. 뭔말이냐면, 비동기 함수는 콜백 함수를 즉시 실행하지 않는다. 비동기 함수가 호출되면, 그 함수는 빠르게 반환되고, 그 안의 콜백 함수는 나중에 실행.
- 비동기 코드는 비동기 함수가 종료된 이후 완료된다

이러한 특성 때문에, 비동기로 동작하는 코드에서의 처리 결과는 **즉시** 받을 수 없다.

외부로 리턴하는 것도, 상위 스코프의 변수에 할당하는 것도 효과 없음!! 비동기 작업이 끝나기 전에 출력될수도 있기 때문에.

```ts
const get = (url) => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    // 이곳은 비동기적으로 호출됩니다.
    return JSON.parse(xhr.response);
  };
};

const response = get('http:~');
console.log(response);
```
위 코드를 보자. response에는 undefined값이 할당된다. 
이 코드에서 중요한 점은 **xhr.onload는 비동기적**이라는 것이다
즉, xhr.onload의 콜백 함수는 get() 함수가 끝난 후에 실행된다. xhr.onload 내부에서 **return JSON.parse(xhr.response)**를 하더라도, 그 값은 get() 함수의 반환값이 되지 않는다.
왜냐하면 비동기 함수의 콜백은 get() 함수가 반환된 "후"에 실행되기 때문!!

결론 : 비동기 함수의 처리 결과에 대한 처리(ex. 출력)는 비동기 함수 내부에서 끝내버려야 한다. 그러기 위해 콜백 함수를 전달하는 것이 일반적.

비동기 처리 결과를 후속처리 하기 위한 비동기 함수. 그 비동기함수의 처리결과를 후속처리하기 위한 또 다른 비동기함수... 하지만 그러다 보면 콜백 함수가 끝없이 중첩되어 **콜백 헬이 발생**한다.

예시1
```ts
function firstTask(callback) {
  setTimeout(() => {
    console.log('First task completed');
    callback();  // 첫 번째 작업이 끝난 후, 두 번째 작업 실행
  }, 1000);
}

function secondTask() {
  setTimeout(() => {
    console.log('Second task completed');
  }, 1000);
}

// 첫 번째 작업이 끝난 후, 두 번째 작업이 실행됨
firstTask(secondTask);
```

출력값은
First task completed
Second task completed

```ts
function firstTask(callback) {
  setTimeout(() => {
    console.log('First task completed');
    callback();
  }, 1000);
}

function secondTask(callback) {
  setTimeout(() => {
    console.log('Second task completed');
    callback();
  }, 1000);
}

function thirdTask(callback) {
  setTimeout(() => {
    console.log('Third task completed');
    callback();
  }, 1000);
}

firstTask(() => {
  secondTask(() => {
    thirdTask(() => {
      console.log('All tasks completed');
    });
  });
});
```

출력값은
First task completed
Second task completed
Third task completed
All tasks completed

또 다른 큰 문제점은 비동기 작업에서 **에러 처리가 곤란**하다는 것이다.
이유는 비동기 작업이 즉시 완료되지 않기 때문. 
비동기 작업은 나중에 완료되므로, 에러를 바로 확인하거나 처리할 수 없다. 이는 에러가 발생하는 시점과 그 에러를 처리하는 시점이 다르기 때문에 발생하는 문제이다.

## 45.2 프로미스의 생성

이러한 콜백함수의 문제점을 극복하기 위해 나온 패턴이 **Promise **다.

Promise는 비동기 처리 상태와 처리 결과를 관리하는 객체다
Promise는 비동기 함수를 수행할 콜백 함수를 인수로 전달받는다.
이 콜백 함수는 resolve 와 reject 함수를 인수로 전달받는다.


프로미스는 비동기 처리가 어떻게 진행되고 있는지의 대한 상태 정보를 제공한다
pending: 비동기 처리 아직 수행 전
fulfilled: 비동기 처리 성공
rejected: 비동기 처리 수행 실패

## 45.3 후속 처리 메서드
프로미스의 처리 결과를 갖고 이제 에러처리를 해야한다.
이를 위해 프로미스는 후속 메서드 then, catch, finally를 제공한다

`모든 후속처리 메서드는 프로미스를 반환한다.
모든 후속처리 메서드는 비동기로 동작한다.`

**then**

fulfilled(첫번째 콜백함수),rejected(두번째 콜백함수) 시 사용.

**catch**

rejected 시 사용. 위에 rejected 시 사용하는 then과 동일하게 동작한다.

그렇다면 무엇을 쓰는 것이 더 좋나요?

=> catch 메서드를 사용하는 것이 더 일반적이고 권장됩니다. catch 메서드를 사용하면 코드가 더 명확해지고, 에러 처리에 관련된 부분이 명시적으로 표현됩니다.

**finally**

fulfilled,rejected 와 상관없이 무조건 한 번 호출된다
이는 모든 걸 다 떠나서 공통적으로 수행해야하는 내용이 있을 때 유용하다

## 45.4 프로미스의 에러 처리

```js
promiseGet("httpi://jsdeepdive/1")
  .then((res) => console.log("success!!"))
  .catch((err) => console.error(err));
```
p.855-856의 예제들 참고.
