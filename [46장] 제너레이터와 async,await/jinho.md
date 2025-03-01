## 46.1 제너레이터란?
코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

제너레이터와 일반 함수의 차이
* 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
* 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다. 
* 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다. 

## 46.2 제너레이터 함수의 정의
* function* 키워드로 선언, 하나 이상의 yield 표현식을 포함한다.
* 제너레이터 함수는 화살표 함수로 정의할 수 없다.

## 46.3 제너리에터 객체
제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 

## 46.4 제너레이터의 일시 중지와 재개
yeild 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다. <br>
=> 일반 함수는 호출 이후 제어권을 함수가 독점하지만 제너레이터는 함수 호출자에게 제어권을 양도하여 필요한 시점에 함수 실행을 재개할 수 있다.<br>

## 46.5 제너레이터의 활용
### 46.5.1 이터러블의 구현
이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

### 46.5.2 비동기 처리
프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현 가능<br>


## 46.6 async / await
제너레이터를 통해 비동기 처리를 동기 처리처럼 동작하도록 구현하는데 코드가 무척이나 장황해지고 가독성도 나빠진다. <br>
=> es8에서 async / await 도입<br>
async/await는 프로미스 기반으로 동작 => async/await를 사용하면 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스 사용가능<br/>
그게 무슨말이냐!<br/>
**Promise**는 비동기 작업의 결과를 나중에 처리할 수 있게 해주는 객체<br/>
then, catch, **finally**는 **Promise**의 후속 메서드로, 비동기 작업이 끝난 후에 실행될 코드를 콜백 함수 형태로 전달함<br/>
예를 들어, then은 성공한 경우, catch는 실패한 경우를 처리<br/>
하지만 이 방식은 콜백 함수를 **then**이나 **catch**에 전달해야 하므로 코드가 중첩되고, 읽기 어려운 경우가 많다.<br/><br/>

async: 함수 앞에 async를 붙이면, 해당 함수는 항상 **Promise**를 반환.<br/>
await: await 키워드를 사용하면 **Promise**가 해결될 때까지 기다린 후, Promise의 값을 반환받을 수 있다

### 46.6.1 async 함수
async함수는 async키워드를 사용해 정의하며 언제나 프로미스를 반환

### 46.6.2 await 키워드
* 프로미스가 비동기 처리가 수행될 때까지 대기하다가 비동기 처리가 수행되면 프로미스가 resolve한 처리 결과를 반환
* await 키워드는 반드시 프로미스 앞에서 사용

### 46.6.3 에러 처리
앞장에서 살펴본 것처럼 에러는 호출자 방향으로 전달된다. <br>
but 비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니기 때문에 
try...catch문을 사용해 에러를 캐치할 수 없다.

async/await는 프로미스를 반환하기에 명시적으로 호출할 수 있기 때문에 호출자가 명확 => try...catch 사용 가능
