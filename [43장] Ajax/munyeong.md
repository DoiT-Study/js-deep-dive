# 43장

## ? Ajax
브라우저가 서버에게 비동기 방식으로 데이터를 요청하고 요청에 의해 서버가 수신한 데이터를 기반으로 웹페이지를 동적으로 갱신하는 프로그래밍 방식

<br>
? Ajax 등장 전 후의 변화

- 불필요한 데이터 통신이 발생하지 않음
- 화면의 전환이 자연스러움
- 서버에 요청을 보낸 이후 블로킹이 발생하지 않음


- 갱신되어야 하는 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
- 변경할 필요가 없는 부분은 다시 렌더링 하지 않기 때문에 화면의 전환이 자연스러움
- 통신이 비동기 방식으로 동작하기 때문에 서버에 요청을 보낸 이후 블로킹이 발생하지 않음

<br>

## JSON
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- 키와 값으로 구성된 순수한 텍스트

<br>
? JSON.stringify
<br>
객체를 JSON 포맷의 문자열로 변환하는 메서드
<br>
직렬화를 위해 사용
<br>
배열도 변환 가능

<br>

? JSON.parse
<br>
JSON 포맷의 문자열을 객체로 변환하는 메서드
<br>
역직렬화를 위해 사용

<br>

## XMLHttpRequest
- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성
- XMLHttpRequest 객체는 HTTP 요쳥 전송과 HTTP 응답 수신을 위한 다양한 프로퍼티와 메서드를 제공

#### HTTP 요청
1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청 초기화

```javascript
xhr.open(method, url[, async]) // 메소드, 요청 전송할 url, 비동기 요청 여부
```
2. XMLHttpRequest.prototype.setRequestHeader로 헤더 값 설정

```javascript
// HTTP 요청 메서드 종류
// GET | POST | PUT | PATCH | DELETE
```
3. XMLHttpRequest.prototype.send 메서드로 요청 전송


<br>

#### HTTP 응답
이벤트 핸들러를 활용하여 서버의 응답이 완료되었는지 확인하고 정상 처리로 판단되었을 경우에는 서버가 전송한 데이터를 취득함
