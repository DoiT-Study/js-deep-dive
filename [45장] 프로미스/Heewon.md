# 45. 프로미스

## 45.5. 프로미스 체이닝

프로미스는 `then`, `catch`, `finally` **후속 처리 메서드**를 통해 **콜백 헬을 해결**한다. 
이 메서드들은 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다.이를 프로미스 체이닝이라 한다. 
이들은 콜백 함수가 반환한 프로미스를 반환하며, 그렇지 않더라도 그 값을 `resolve` 또는 `reject`하여 프로미스를 생성해 반환한다.

```jsx
const url = 'https://jsonplaceholder.typicode.com';

promiseGet(`${url}/posts/1`)
    .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.log(err));
```
다만 프로미스도 콜백 패턴을 사용하고 있고, 콜백 패턴은 가독성이 좋지 않기 때문에 ES8부터 도입된 `async`/`await`을 사용하는 것이 좋다.
async,await을 쓰면 프로미스의 후속 처리 메서드 없이,
마치 **동기처리처럼** 프로미스가 처리 **결과를 반환**하도록 구현할 수 있다!

```jsx
const url = 'https://jsonplaceholder.typicode.com';

(async () => {
    const { userId } = await promiseGet(`${url}/posts/1`);

    const { userInfo } = await promiseGet(`${url}/users/${userId}`);

    console.log(userInfo)
})();
```

## 34.6. 프로미스의 정적 메서드

### 45.6.1. `promise.resolve` / `Promise.reject`

`promise.resolve` 메서드는 인수로 전달받은 값을 '해결'한다.

```jsx
const resolvedPromise = Promise.resole([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```

`Promise.reject` 메서드는 인수로 전달받은 값을 '거절'하여 에러를 발생시킨다.

```jsx
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.log); // Error: Error!
```

### 45.6.2. `Promise.all`

`Promise.all` 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다. 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받고, 전달받은 프로미스가 모두 `fulfilled` 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 생성한다.

`resolve`의 경우 **처리 순서가 보장**되며,  <br/>
`reject`의 경우 전달받은 프로미스가 하나라도 `reject`되면 나머지 프로세스가 `fulfilled`되는 것을 **기다리지 않고 종료**한다.
전달받은 이터러블의 요소가 프로미스가 아닌 경우 `resolved` 메소드를 사용해 프로미스로 래핑한다.

```jsx
const requestData1 = () =>
    new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
    new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
    new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1(), requestData2(), requestData3()])
    .then(console.log) // [1, 2, 3]
    .catch(console.error);
```

```jsx
Promise.all([
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 1')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 2')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 3')), 3000))
]).then (console.log).catch(console.log); // Error: Error 3
```

### 45.6.3. `Promise.race`

`Promise.all` 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 
그러나 모든 프로미스를 기다리는 것이 아니라 **가장 먼저 `fulfilled` 상태가 된 프로미스의 처리 결과를 `resolve`하는 새로운 프로미스를 반환**한다.<br/> `reject`의 경우 `Promise.all` 메서드와 동일하게 처리된다.

```jsx
Promise.race([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
    new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
    new Promise(resolve => setTimeout(() => resolve(3), 1000)) // 3
]).then (console.log) // 3
    .catch(console.log);
```

```jsx
Promise.race([
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 1')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 2')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error 3')), 3000))
]).then (console.log).catch(console.log); // Error: Error 3
```

### 45.6.4. `Promise.allSettled`

`Promise.allSettled` 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 전달받는 프로미스는 모두 `settled` (`fulfilled` 또는 `rejected`) 가 되어 배열로 반환된다.

- 프로미스가 `fulfilled`인 경우 비동기 처리 상태를 나타내는 `status` 프로퍼티와 처리 결과를 나타내는 `value` 프로퍼티를 갖는다.
- 프로미스가 `rejected`인 경우 비동기 처리 상태를 나타내는 `status` 프로퍼티와 에러를 나타내는 `reason` 프로퍼티를 갖는다.

```jsx
Promise.allSettled([
    new Promise(resolve => setTimeout(() => resolve(1), 2000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log);
/*
    {status: "fulfilled", value: 1},
    {status: "rejected", reason: Error: Error! at <anonymous>:3:54}
*/
```

## 45.7. 마이크로태스크 큐

```ts
setTimeout(() => console.log(1),0);

Promise.resolve()
    .then(()=>console.log(2));
    .them(()=>console.log(3));
```
출력순서는 2->3->1이다.
프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장된다.
마이크로태스크 큐는 태스크 큐보다 우선순위가 높다. 마이크로태스크를 비운 후에 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.

- 마이크로태스크 큐 : 프로미스의 후속 처리 메서드의 콜백 함수를 임시 저장
- 태스크 큐 : 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러 저장

## 45.8. `fetch`

HTTP 요청 전송 기능을 제공하며 프로미스를 지원하는 클라이언트 사이드 Web API이다. HTTP 요청을 전송할 URL, HTTP 요청 메서드, HTTP 요청 헤어, 페이로드 등을 설정한 객체를 전달하며 HTTP 응답을 나타내는 `Response` 객체를 래핑한 프로미스 객체를 반환한다. 

`fetch` 함수를 사용할 때는 에러 처리에 주의해야 한다. `fetch` 함수가 반환하는 프로미스는 HTTP 에러가 발행해도 에러를 `reject`하지 않고 `ok` 객체를 `false`로 설정한 `Response` 객체를 `resolve`한다. 요청이 완료되지 못한 경우에만 `reject`한다. 따라서 `ok` 객체를 확인해 명시적으로 에러를 처리해야 한다.

HTTP 요청은 `GET` - `POST` - `PATCH` - `DELETE` 순서로 진행된다.
