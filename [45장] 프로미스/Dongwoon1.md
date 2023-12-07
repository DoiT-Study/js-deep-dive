# 45.5 프로미스 체이닝
원래 비동기 처리를 위한 콜백 함수는 콜백 헬을 불러일으킨다는 문제점이 있다.

그러나 프로미스의 then, catch, finally 메서드를 사용한다면 해결 가능하다.
```javascript
const url = 'https://www.naver.com/';

promisGet('${url}/posts/1')
.then(({ userId }) => promisGet('${url}/users/${userId}'))
.then(userInfo => console.log(userInfo))
.catch(err => console.log(err));
```
then, then, catch 순으로 메서드를 호출하였다.

이렇게 연속적으로 호출이 가능한 것을 프로미스 체이닝이라 한다.

또한, 이 메서드들은 콜백 함수가 반환한 프로미스를 반환한다는 공통점이 있다.

즉, 프로미스는 프로미스 체이닝을 통해 후속 처리를 하므로 콜백 헬이 발생하지 않는다.

# 45.6 프로미스의 정적 메서드
프로미스는 후속 처리 메서드 외에도 5가지의 정적 메서드를 제공한다.

## 45.6.1 Promis.resolve / Promise.reject
이 메서드들은 이미 존재하는 값을 래핑해 프로미스를 생성한다.

```javascript
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```
전달받은 값을 resolve하는 프로미스 생성
```javascript
const rejectedPromise = Promise.reject(new Error('Error?'));
rejectedPromise.catch(console.log); // Error : Error?
```
전달받은 값을 reject하는 프로미스 생성

## 45.6.2 Promise.all
이 메서드는 여러 개의 비동기 처리를 병렬로 처리할 때 사용된다.

이 메서드는 전달받은 모든 프로미스가 fullfilled 상태가 되면 종료되고 결과를 배열로 저장해 새로운 프로미스를 반환해낸다.

또한, 결과로 나온 배열은 처리 순서가 보장되어 첫 번째 프로미스가 나중에 fullfilled 되어도 차례대로 배열에 저장한다.

만약 프로미스들 중 하나라도 rejected된다면 즉시 종료한다.
```javascript
const requestdata1 = () =>
 new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestdata2 = () =>
 new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestdata3 = () =>
 new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([requestdata1(), requestdata2(), requestdata3()])
.then(console.log) // [1, 2, 3]
.catch(console.error);
```
## 45.6.3 Promise.race
이 메서드는 Promise.all 메서드와 동일하지만 모든 프로미스가 fullfilled 되기를 기다리는게 아닌 가장 먼저 fullfilled된 프로미스를 resolve하는 새로운 프로미스를 반환한다.

하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 반환한다.

## 45.6.4 Promise.allsettled
이 메서드는 .all, .race와 유사하나 모두 settled상태가 되면 결과를 배열로 반환한다.

이때, settled상태는 fullfilled or rejected 상태를 의미한다.

따라서, 결과에는 인수로 전달받은 모든 프로미스들의 처리 결과가 포함되어 있다.

# 45.7 마이크로태스크 큐
```javascript
setTimeout(() => console.log(1), 0);

Promise.resolve()
.then(() => console.log(2))
.then(() => console.log(3));
```
이 코드는 1, 2, 3 순이 아닌 2, 3, 1순으로 동작한다.

이 이유는 마이크로태스크 큐 때문인데 프로미스의 후속 처리 메서드의 콜백 함수가 일시 정지되기 때문이다.

그래서 일반 콜백함수와 이벤트 핸들러도 태스크 큐에서 정지하지만 마이크로태스크 큐가 더 우선순위가 높다.

# 45.8 fetch
fetch 함수는 HTTP 요청 전송 기능을 하는 클라이언트 사이드 웹 API이다.

또한, 프로미스를 제공하기 때문에 비동기 처리를 위한 콜백 함수에 대한 문제점은 고려 대상이 아니다.

fetch 함수는 HTTP 응답을 나타내는 respose 객체를 래핑한 Promise 객체(프로미스)를 반환한다.

fetch 함수가 반환하는 프로미스는 HTTP 에러가 발생해도 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 resolve한다.

그래서 네트워크 장애 등의 에러로 인해 요청이 완료되지 못한 경우에만 reject한다.

fetch 함수를 통한 다양한 HTTP 요청이 존재한다.
1. Get 요청
2. POST 요청
3. PATCH 요청
4. DELETE 요청
