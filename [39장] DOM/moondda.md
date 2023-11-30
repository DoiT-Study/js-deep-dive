DOM?

- Document Object Model
- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메소드를 제공하는 트리 자료구조다.

  39.1장 노드

  39.1.1 HTML 요소와 노드 객체

ex) `<div class="greeting">Hello</div>`

요소는 요소 노드로 div
어트리뷰트는 어트리뷰트 노드로 class="greeting"
텍스트 콘텐츠는 텍스트 노드로 Hello

HTM문서는 요소의 집합으로 이뤄지며 중첩 관계에 의해 계층적인 관계가 형성된다
=> 트리 자료구조가 된다~

이때 이 트리 자료구조를 DOM이라고 한다. DOM 트리라고 부르기도 한다.

39.1.2 노드 객체의 타입

p.679 참고

```
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Google Finance</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

대표적 4가지 타입

문서 노드 :

- DOM트리의 최상위에 존재하는 루트 노드.
- document 객체를 가르킨다.
- window.document를 참조할 수 있음
- DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다 -> 요소 어트리뷰트 텍스트 노드에 접근하려면 문서 노드를 통해야함!!

요소 노드 :

- 문서의 구조. 중첩 관계를 통해 정보를 구조화한다

어트리뷰트 노드 :

- 요소 노드와 연결되어 있다. 즉 어트리뷰트를 참조하거나 변경하려면 요소 노드에 먼저 접근해야함

텍스트 노드 :

- 요소 노드의 자식 노드
- DOM 트리의 최종단

39.1.3

DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 노드 객체는 자신의 부모형제자식을 탐색할 수 있고, 어트리뷰트와 텍스트를 조작할 수 있다.

프론트엔드 개발자에게 HTML은 단순히 태그와 어트리뷰트를 선언적으로 배치하여 뷰를 구성하는 것 이상의 의미를 갖는다. HTML과 DOM을 연관지어 바라봐야 한다.

39.2 요소 노드 취득

id
getElementBtId

태그
getElementByTagName

class
getElementByClassName

CSS 선택자
document.querySelectorAll

공백 텍스트 노드

- HTML 요소 사이의 스페이스,탭,개행 등의 공백은 텍스트 노드를 생성한다.
- 따라서 노드를 탐색할 때 공백 문자가 생성한 공백 텍스트 노드에 주의해야한다
  -> 모든 공백 문자를 제거하면 되는 거 아닌가요??
  - 그럴게 되면 가독성이 떨어지므로 권장하지 않는다
