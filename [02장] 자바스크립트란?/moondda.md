## 2.1 자바스크립트의 탄생

웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정했다

이름 : 모카(Mocha) -> 라이브스크립트(LiveScript) -> 자바스크립트(JavaScript)


## 2.2 자바스크립트의 표준화

1996년 8월, 마이크로소프트는 자바스크립트의 파생 버전인 JScript를 인터넷 익스플로러 3.0에 탑재했다.

문제점 : 표준화가 아니고 적당히 호환되었다. 넷스케이프 커뮤니케이션즈(JavaScript)와 마이크로소프트(JScript)가 자사 브라우저의 시장 점유율을 높이기 위해 자사 브라우저에서만 동작하는 기능을 추가하기 시작함
-> `크로스 브라우싱 이슈 발생`

1996년 11월, 넷스케이프 커뮤니케이션즈는 ECMA 인터내셔널에 자바스크립트의 표준화를 요청한다

1997년 7월, 표준화된 자바스크립트 초판 사양이 완성되고, 상표권 문제로 자바스크립트는 ECMAScript로 명명된다.

2015년, ES6이 공개되었고, 이는 let/const 키워드, 화살표 함수, 클래스, 모듈 등의 도입으로 큰 변화가 있었다

  
## 2.3 자바스크립트 성장의 역사

초창기 자바스크립트: 웹페이지의 보조적인 기능을 수행하기 위한 한정적인 용도
로직은 웹서버에서 실행, 브라우저는 서버로부터 받은 HTML, CSS를 단순히 렌더링

### Ajax
XMLHttpRequest라는 이름으로 등장
자바스크립트를 이용하여 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있게 되었다.

전: 웹페이지에서 변경할 필요가 없는 부분도 다시 처음부터 렌더링
후: 서버로부터 필요한 데이터만 전송받아 변경해야 하는 부분만 한정적으로 렌더링

### jQuery
DOM 더욱 쉽게 제어 가능
크로스 브라우징 이슈 해결
자바스크립트보다 배우기 쉽고 직관적이다

### V8 자바스크립트 엔진
웹 어플리케이션 프로그래밍 언어로서의 가능성이 있다!
-> 더 빨리 동작하는 자바스크립트 엔진이 필요하다
-> V8 자바스크립트 엔진
-> 과거 웹 서버에서 수행되던 로직들이 클라이언트로 이동했다

### Node.js
구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경<br/><br/>
**브라우저의 자바스크립트 엔진에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서도 동작할 수 있도록 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경**

- 비동기 I/O 지원
- 단일 스레드 이벤트 루프 기반 동작
- I/O가 빈번하게 발생하는 SPA에 적합
  
### SPA 프레임워크

CBD(Component based development) 방법론을 기반으로 하는 SPA 대중화 <br/>
ex) Angular React Vue.js Svelte 

## 2.4 자바스크립트와 ECMAScript

## 2.5 자바스크립트의 특징 

- 웹 브라우저에서 동작하는 유일한 프로그래밍 언어
- 인터프리터 언어
- 명령형 함수형 프로토타입 기반 객체지향 프로그래밍 지원
  <br/> -> `멀티 패러다임 프로그래밍 언어`
  
