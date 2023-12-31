# 39장 DOM

DOM : DOM은 웹 페이지, 즉 HTML 문서를 계층적 구조와 정보로 표현하며, 이를 제어할 수 있는 프로퍼티와 메서드를 제공하는 트리 자료구조이다.

# 39.6 DOM 조작

> DOM 조작: 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 or 교체하는 것

성능에 영향을 주기에 주의하여 사용

## innerHTML

> Element.prototype.innerHTML : setter, getter 모두 존재하는 접근자 프로퍼티

> 요소 노드의 HTML 마크업을 취득 or 변경
> (마크업 : 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어의 한 가지)

```js
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    // #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.

    console.log(document.getElementById('foo').innerHTML);
    // "Hello <span>world!</span>"
  </script>
</html>
```

> 문자열 할당 시 HTML 마크업을 파싱하여 요소노드의 자식 노드로 DOM에 반영된다.

```js
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>

    //HTML 마크업이 하싱되어 요소노드의 자식 노드로 DOM에 반영된다.
    document.getElementById('foo').innerHTML = 'Hi <span> there!</span>';
  </script>
</html>
```

> 단점:

1.  마크업 내에 악성 자바스크립트 코드를 파싱 과정에서 그대로 실행할 가능성이 있기에 innerHTML 프로퍼티를 사용하여 사용자로부터 입력 받은 데이터를 그대로 할당하는 것은 "크로스 사이트 스크립팅 공격"에 취약하므로 위험하다.

2.  삼입될 위치 지정불가

3.  복잡하지 않은 요소를 새롭게 추가할때는 유용하지만 기존 요소를 유지하여 추가하거나 위치를 지정할때는 좋지 않다.

크로스 사이트 스크립팅: '공격자'의 웹사이트에서 피해자가 친숙하다고 느끼는 웹사이트에 악성 스크립트를 주입하는 행위

## insertAdjacentHTML 메서드

> 기존요소를 제거하지 않으면서 위치를 지정해 새로운 요소 삽입

> 첫 번째 인수로 전달한 위치에 두 번째 인수로 전달한 HTML 마크업 문자열을 "파싱"하여 추가

> 첫번째 인수로 올 수 있는 문자열

1. beforebegin
2. afterbegin
3. beforeend
4. afterend

```js
<!DOCTYPE html>
<html>
  <body>
    <!-- beforebegin -->
    <div id="foo">
      <!-- afterbegin -->
      text
      <!-- beforeend -->
    </div>
    <!-- afterend -->
  </body>
  <script>
    const $foo = document.getElementById('foo');

    $foo.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>');
    $foo.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>');
    $foo.insertAdjacentHTML('beforeend', '<p>beforeend</p>');
    $foo.insertAdjacentHTML('afterend', '<p>afterend</p>');
  </script>
</html>
```

> 단점 : innerHTML과 마찬가지로 크로스 사이트 스크립팅 공격에 취약

## 노드 생성과 추가

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 1. 요소 노드 생성
    const $li = document.createElement('li');

    // 2. 텍스트 노드 생성
    const textNode = document.createTextNode('Banana');

    // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
    $li.appendChild(textNode);

    // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($li);
  </script>
</html>
```

요소 노드 생성 Document.prototype.createElement(tagName) : 요소 노드를 생성하여 반환

텍스트 노드 생성 Document.prototype.createTextNode(text) : 텍스트 노드를 생성하여 반환

텍스트 노드를 요소 노드의 자식 노드로 추가 : 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.

요소 노드를 DOM에 추가: Node.prototype.appendChild 메서드를 사용해 텍스트 노드와 부자 관계로 연결한 요소 노드를 #fruits 요소 노드의 마지막 자식 요소로 추가한다.

## 복수의 노드 생성과 주기

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.appendChild($li);
    });
  </script>
</html>
```

DOM에 추가할 수 있으나 DOM이 3번 변경되므로 리플로우와 리페인트 3번 실행 -> 비효율적.

-> DocumentFragment를 사용

DocumentFragment

1. 부모 노드가 없어서 기존 DOM과는 별도로 존재한다
2. 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM 에 추가하기 위한 용도로 사용
3. DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가

-> 리플로우와 리페인트 1번 발생 == 성능 향상

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
      $fragment.appendChild($li);
    });

    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
  </script>
</html>
```

```
Apple
Banana
Orange
```

## 노드 삽입

> 마지막 노드로 추가
> Node.prototype.appendChild

> 지정한 위치에 노드 삽입
> Node.prototype.insertBefore(newNode, childNode)
> 첫 번째 인수로 전달받은 도드를 두 번쨰 인수로 전달받은 노드 앞에 삽입

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 요소 노드 생성
    const $li = document.createElement('li');

    // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
    $li.appendChild(document.createTextNode('Orange'));

    // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    document.getElementById('fruits').appendChild($li);

    // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
    $fruits.insertBefore($li, $fruits.lastElementChild);
  </script>
</html>
```

```
Apple
Banana
Orange
```

## 노드 이동

> 이미 존재하는 노드를 appendChid 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 이미 존재하는 요소 노드를 취득
    const [$apple, $banana, ] = $fruits.children;

    // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
    $fruits.appendChild($apple); // Banana - Orange - Apple

    // 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
    $fruits.insertBefore($banana, $fruits.lastElementChild);
    // Orange - Banana - Apple
  </script>
</html>
```

```
Orange
Banana
Apple
```

## 노드 복사

> Node.prototype.cloneNode([deep: true | false]): 노드의 사본을 생성하여 반환

if (deep에 true를 인수로 전달) -> 노드를 깊이 복사 모든 자손 노드가 포함된 사본을 생성

else if( deep false를 인수로 전달하거나 생략) -> 노드를 얕게 복사 노드 자신만의 사본을 생성, 얕은 복사이니 자손 노드도 복사하지 않으므로 텍스트 노드도 없다.

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');
    const $apple = $fruits.firstElementChild;

    // 요소를 생략했기에 $apple 요소를 얕게 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성된다.
    const $shallowClone = $apple.cloneNode();
    // 사본 요소 노드에 텍스트 추가
    $shallowClone.textContent = 'Banana';
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($shallowClone);

    // 요소에 true가 포함되었기에 #fruits 요소를 깊게 복사하여 모든 자손 노드가 포함된 사본을 생성
    const $deepClone = $fruits.cloneNode(true);
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($deepClone);
  </script>
</html>
```

```
Apple
Banana
    Apple
    Banana
```

## 노드 교체

> Node.prototype.replaceChild(newChild, oldChild): 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다

1. newChild: 교체할 새로운 노드를 인수로 전달
2. oldChild: 이미 존재하는 교체될 노드를 인수로 전달

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 기존 노드와 교체할 요소 노드를 생성
    const $newChild = document.createElement('li');
    $newChild.textContent = 'Banana';

    // #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
    // 이후 oldChild 노드는 DOM에서 제거
  </script>
</html>
```

```
Banana
```

## 노드 삭제

Node.prototype.removeChild(child): child 매개변수에 인수로 전달한 노드를 DOM에서 삭제

```js
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // #fruits 요소 노드의 마지막 요소를 DOM에서 삭제
    $fruits.removeChild($fruits.lastElementChild);
  </script>
</html>
```

```
Apple
```

# 39.7 어트리뷰트

어트리뷰트(attributes) : html 문서 안에서의 정적인(바뀌지 않는) 속성 그 자체

attribute 는 html 문서에서 elements 에 추가적인 정보를 넣을 때 사용되는 요소이다.

예를 들어, `<div class= ‘my-class’></div>`라는 ‘my-class’라는 클래스 속성을 가진 div 요소에서 div 는 element(요소) 이고 class 는 attribute(속성) 가 되며 ‘my-class’ 는 class attribute의 value(값)가 된다.

## 어트리뷰트 노드와 attributes 프로퍼티

HTML 요소는 여러 개의 어트리뷰트(속성)를 가질 수 있다.

모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NameNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.

```js
<input id="user" type="text" value="ungmo2">
```

## HTML 어트리뷰트 조작

1. Element.prototype.getAttribute(attributeName) : 값 참조
2. Element.prototype.setAttribute(attributeName,attributeValue) : 값 변경
3. Element.prototype.hasAttribute(attributeName): 특정 HTML 어트리뷰트 존재 확인
4. Element.prototype.removeAttribute(attributeName) : 특정 HTML 어트리뷰트를 삭제

```js
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트 값을 취득
    const inputValue = $input.getAttribute('value');
    console.log(inputValue); // ungmo2

    // value 어트리뷰트 값을 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo

    // value 어트리뷰트의 존재 확인
    if ($input.hasAttribute('value')) {
      // value 어트리뷰트 삭제
      $input.removeAttribute('value');
    }

    // value 어트리뷰트가 삭제되었다.
    console.log($input.hasAttribute('value')); // false
  </script>
</body>
</html>
```

## HTML 어트리뷰트 vs DOM 프로퍼티

> 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 DOM 프로퍼티가 존재한다.

DOM 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티이다.

> HTML 어트리뷰트의 역할

1. HTML 요소의 초기 상태를 지정 and 초기 상태 관리
2. 변하지 않음(정적임)

> DOM 프로퍼티의 역할

1. 요소 노드의 최신 상태 관리
2. 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지

- attribute와 property를 구분하는 차이 또는 차이점이 무엇일까?

> attribute는 html document/file 안에서 property 는 html DOM tree안에서 존재합니다. 이것이 뜻하는 것은 attribute는 정적으로 변하지 않고 property는 동적으로 그 값이 변할 수 있다는 것을 내포하고 있습니다. 예를 들어 체크박스 태그가 있을 때 유저가 체크박스에 체크를 하면 attribute의 상태는 변하지 않지만 property의 상태는 checked로 변하게 됩니다.
> https://medium.com/hexlant/attribute-%EC%99%80-property-%EC%9D%98-%EC%B0%A8%EC%9D%B4-c6f1c91ba91

1. HTML 어트리뷰트와 DOM 프로퍼티는 1:1으로 대응하지만 but 언제나 대응하는 것은 아니다.

- id 어트리뷰트와 id 프로퍼티는 1:1로 대응하며, 동일한 값으로 연동한다.
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응한다. 하지만 value 어트리뷰트는 초기 + 상태를 value 프로퍼티는 최신 상태를 갖는다.
- class 어트리뷰트는 className, classList 프로퍼티와 1:1 대응한다.
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
- td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
- textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
- 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다.(maxlength → maxLength)

2. HTML 어트리뷰트는 언제나 문자열인 반면 DOM 프로퍼티는 문자열이 아닐 수도 있다.

```js
<!DOCTYPE html>
<html>
<body>
  <input type="checkbox" checked>
  <script>
    const $checkbox = document.querySelector('input[type=checkbox]');

    // getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
    console.log($checkbox.getAttribute('checked')); // ''

    // DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
    console.log($checkbox.checked); // true
  </script>
</body>
</html>
```

## data 어트리뷰트와 dataset 프로퍼티

> data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다. data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

- dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환

HTMLElement.dataset : data 어트리뷰트의 값(value) 취득 or 변경

```js
<!DOCTYPE html>
<html>
<body>
  <ul class="users">
    <li id="1" data-user-id="7621" data-role="admin">Lee</li>
    <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
  </ul>
  <script>
    const users = [...document.querySelector('.users').children];
    console.log(users)
    // user-id가 '7621'인 요소 노드를 취득한다.
    const user = users.find(user => user.dataset.userId === '7621');
    // user-id가 '7621'인 요소 노드에서 data-role의 값을 취득한다.
    console.log(user.dataset.role); // "admin"

    // user-id가 '7621'인 요소 노드의 data-role 값을 변경한다.
    user.dataset.role = 'subscriber';
    // dataset 프로퍼티는 DOMStringMap 객체를 반환한다.
    console.log(user.dataset); // DOMStringMap {userId: "7621", role: "subscriber"}
  </script>
</body>
</html>
```

# 39.8 스타일

## 인라인 스타일 조작

HTMLElement.prototype.style : setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.

```js
<!DOCTYPE html>
<html>
<body>
  <div style="color: red">Hello World</div>
  <script>
    const $div = document.querySelector('div');

    // 인라인 스타일 취득
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }

    // 인라인 스타일 변경
    $div.style.color = 'blue';

    // 인라인 스타일 추가
    $div.style.width = '100px';
    $div.style.height = '100px';
    $div.style.backgroundColor = 'yellow';
  </script>
</body>
</html>
```

style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환

CSS 프로퍼티는 케밥 케이스를 따른다. 이에 대응하는 CSSStyleDeclaration객체의 프로퍼티는 카멜 케이스를 따른다.

## 클래스 조작

"."으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일을 변경할 수도 있다. 이때 HTML 요소의 class 어트리뷰트를 조작하려면 ckass 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티를 사용한다.

### className

- setter, getter 모두 존재하는 접근자 프로퍼티이다.
- HTML 요소의 class 어트리뷰트 값을 취득 또는 변경한다.
- 문자열을 반환하므로 여러 개의 클래스를 다루기에는 불편하다.

```js
<!DOCTYPE html>
<html>
<head>
  <style>
    .box {
      width: 100px; height: 100px;
      background-color: antiquewhite;
    }
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<body>
  <div class="box red">Hello World</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소의 class 어트리뷰트 값을 취득
    console.log($box.className); // 'box red'

    // .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
    $box.className = $box.className.replace('red', 'blue');
  </script>
</body>
</html>
```

### classList

- class 어트리뷰트의 정보를 담은 DOMToketList 객체를 반환한다.
- 컬랙션 객체, 유사 배열 객체로서 유사 배열 객체이면서 이터러블이다.
- 다양한 메서드를 제공한다.

이터러블 : 이터러블(interable)이란 자료를 반복할 수 있는 객체를 말하는 것이다.

```js
<!DOCTYPE html>
<html>
<head>
  <style>
    .box {
      width: 100px; height: 100px;
      background-color: antiquewhite;
    }
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<body>
  <div class="box red">Hello World</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
    // classList가 반환하는 DOMTokenList 객체는 HTMLCollection과 NodeList와 같이
    // 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) 객체다.
    console.log($box.classList);
    // DOMTokenList(2) [length: 2, value: "box blue", 0: "box", 1: "blue"]
  </script>
</body>
</html>
```

1. add: 인수로 전달한 1개 이상의 문자열을 calss 어트리뷰트 값으로 추가한다.
2. remove: 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 삭제한다.
3. contains: 인수로 전달한 문자열과 일치하는 클래스가 포함되어 있는지 확인한다.
4. replace: 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 클래스 변경한다.
5. toggle: 일치하는 클래스가 있으면 제거하고 존재하지 않으면 추가한다.

## 요소에 적용되어 있는 CSS 스타일 참조

window.getComputedStyle(element[, pseudo]) : 첫 번째 인수(element)로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환한다.

```js
<html>
<head>
  <style>
    body {
      color: red;
    }
    .box {
      width: 100px;
      height: 50px;
      background-color: cornsilk;
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <div class="box">Box</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
    const computedStyle = window.getComputedStyle($box);
    console.log(computedStyle); // CSSStyleDeclaration

    // 임베딩 스타일
    console.log(computedStyle.width); // 100px
    console.log(computedStyle.height); // 50px
    console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
    console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

    // 상속 스타일(body -> .box)
    console.log(computedStyle.color); // rgb(255, 0, 0)

    // 기본 스타일
    console.log(computedStyle.display); // block
  </script>
</body>
</html>
```

getComputedStyle 메서드의 두 번째 인수(pseudo)로 :after, :before와 같은 의사 요소를 지정하는 문자열을 전달할 수 있다. 의사 요소가 아닌 일반 요소의 경우 두 번째 인수는 생략한다.

```js
<!DOCTYPE html>
<html>
<head>
  <style>
    .box:before {
      content: 'Hello';
    }
  </style>
</head>
<body>
  <div class="box">Box</div>
  <script>
    const $box = document.querySelector('.box');

    // 의사 요소 :before의 스타일을 취득한다.
    const computedStyle = window.getComputedStyle($box, ':before');
    console.log(computedStyle.content); // "Hello"
  </script>
</body>
</html>
```
