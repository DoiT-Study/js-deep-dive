# 39장 DOM
DOM은 HTML문서의 정보를 표현하는 API로서 프로퍼티와 메서드를 제공하는 트리 자료구조 이다.

## 39.1 노드
### 39.1.1 HTML요소와 노드 객체
HTML요소는 HTML 문서를 구성하는 개별적인 요소이다.

예를 들면, 시작 / 종료 태그, 속성 이름, 속성 값, 내용값 등이 있다.

이때, 이 요소들은 DOM을 구성하는 요소 노드 객체로 변환된다. 

트리 자료구조는 노드들의 계층 구조이며 부모 노드와 자식 노드로 구성된다.

즉, 비선형 구조이며 시작점은 가장 위에 있는 최상위 노드(루트 노드)로부터 시작되어 자식 노드로 나아간다.

### 39.1.2 노드 객체의 타입
노드 객체는 상속 구조를 갖는데 여기서 노드 객체는 12개의 종류를 가진다.

***문서 노드***

문서 노드는 루트 노드로서 document 객체를 가리킨다. 

document 객체는 HTML 문서 전체를 의미하며 전역 객체 window를 바인딩하여 HTML 문서 하나 당 document 객체는 하나이다.

***요소 노드***

요소 노드는 HTML의 요소를 가리키는 객체로 문서의 구조를 표현한다.

대표적으로 HTML에서의 태그를 가리킨다.

***어트리뷰트 노드***

어트리뷰트 노드는 HTML에서의 속성을 가리키는 객체이며 어트리뷰트가 지정되어 있는 요소 노드와 연결되어 있다.

따라서, 어트리뷰트 노드는 부모 노드가 없이 오로지 요소 노드와 연결되어 있다.(이때 형제 관계는 X)

***텍스트 노드***

텍스트 노드는 HTML에서 텍스트를 가리켜 문서의 정보나 내용을 표현한다.

텍스트 노드는 요소 노드의 자식 노드이고 동시에 자식이 없는 리프 노드이다.

가장 마지막으로 접근되는 노드이므로 요소 노드에 먼저 접근해야 참조가 가능하다.

### 39.1.3 노드 객체의 상속 구조

노드 객체는 DOM에서의 API를 사용할 수 있기 때문에 자신의 부모, 자식을 탐색, 조작 가능하다.

노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.

또한, 문서 노드는 HTMLDocument 인터페이스를 상속받고 어트리뷰트 노드는 Attr 인터페이스, 텍스트 노드는 CharacterData 인터페이스, 요소 노드는 Element 인터페이스를 상속받는다.

따라서, 여기서의 input 요소 노드 객체는 프로토타입 체인 내의 모든 프로퍼티나 메서드를 상속받아 사용 가능하다.

모든 노드 객체는 공통적으로 트리 탐색 기능, 노드 정보 제공 기능이 필요한데 이 기능을 Node 인터페이스가 보충해준다.

노드 객체는 공통된 기능일 수록 상위의 프로토타입 체인에, 특수한 기능일수록 하위에 구축되어 프로퍼티와 메서드를 제공한다. 

## 39.2 요소 노드 취득

HTML의 스타일, 내용을 조작하기 위해서는 요소 노드 취득이 우선이다.

### 39.2.1 id를 이용한 요소 노드 취득

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <li id = "apple">Apple</li>
        <li id = "melon">Melon</li>
        <li id = "orange">Orange</li>
    </ul>
        <script>
        const $elem = document.getelement('melon'); // id값이 melon인 요소 노드를 탐색해 반환
        $elem.style.color = 'red';
        </script>
</body>
</html>
```
위 코드에서 id값은 문서 내에서 유일한 값으로 여러 개의 값을 가질 수 없다.

그러나, 만약 id의 값이 중복될 경우에는 첫 번째 요소 노드만 반환한다. 

이외에도 없는 id 값을 반환하려한다면 null 값이 반환된다.

id 값을 부여한다면 동일한 이름의 전역 벼수가 선언되고 해당 노드 객체가 할당된다.

### 39.2.2 태그 이름을 이용한 요소 노드 취득
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <li id = "apple">Apple</li>
        <li id = "melon">Melon</li>
        <li id = "orange">Orange</li>
    </ul>
        <script>
        const $elem = document.getelement('li'); // 태그 이름이 li인 요소 노드들을 모두 반환
        $elem.style.color = 'red'; // 모든 요소 노드의 style.color 프로퍼티 값이 변경된다.
        </script>
</body>
</html>
```
만약 모든 요소 노드를 취득하려면 특정 메서드의 인수로 '*'를 전달하면 된다. 
### 39.2.3 class를 이용한 요소 노드 취득
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <li class = "fruit apple">Apple</li>
        <li class = "fruit melon">Melon</li>
        <li class = "fruit orange">Orange</li>
    </ul>
        <script>
        const $elem = document.getelement('fruit'); //class 값이 fruit인 요소 노드를 반환한다. 
        const $elem123 = document.getelement('fruit apple'); //class 값이 fruit apple인 요소 노드를 반환한다. 
        </script>
</body>
</html>
```

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul id = "fruits">
        <li class = "apple">Apple</li>
        <li class = "melon">Melon</li>
        <li class = "orange">Orange</li>
    </ul>
        <script>
        const $elem = document.getelement('fruits'); 
        const $elem123 = $elem.getelementbyclassname('melon');
         // id값이 fruits인 요소 노드 중에서도 class 값이 melon인 요소 노드를 탐색해 반환한다. 
        </script>
</body>
</html>
```
### 39.2.4 css 선택자를 이용한 요소 노드 취득

```HTML
*{
    color : red;
} //모든 요소 선택

.노랑 {
    color :yellow;
} // class값이 노랑인 요소를 선택

#초록 {
    color :green;
} id값이 초록인 요소를 선택

ul > h5 {
    color : blue;
} // 인접 형제 선택자 : ul 하위에 있는 직계 자손 h5를 선택

ul li {
    color : purple;
} // 자식 선택자

ul ol {
    color : purple;
} // 자식 선택자

li h5 {
    color : brown;
}

a:visited {
    color : gray;
} // 가상 요소 선택자
```

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

Element.prototype.matches 메서드는 css선택자를 사용해 요소 노드를 취득할 수 있는지를 확인한다.

### 39.2.6 HTMLCollection과 Nodelist

getElementByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체를 살아있는 객체라고 부른다.


p.695의 예제를 참고하면  class값이 red인 요소 노드를 모드 취득한 후 다시 반복문을 통해 class값을 blue로 바꾼다.

여기서 두번째 li 요소만 글씨가 빨간색으로 출력되는 오류가 발생하게 된다.

그 이유는 HTMLCollection이 살아있는 객체이므로 반복문에 나타난다.

따라서 필요한 요소를 무시할 수 있다는 부작용이 있기 때문에 querySelectorAll 메서드를 사용할 수 있다.

이 메서드는 NodeList 객체를 반환하고 이는 실시간으로 상태를 반영하지 않는 객체이다.

따라서, 과거의 정적 상태를 유지하는 객체로서 동작한다.

설명한 두 객체는 모두 부작용이나 사용하기 까다롭기 때문에 배열로 변환하여 사용하는 것을 권장한다.

## 39.3 노드 탐색
요소 노드 취득을 거친 후, 노들를 옮겨 다니며 탐색하고 반환되게 된다.

```HTML
<ul id = "fruits">
  <li class = "apple">Apple</li>
  <li class = "melon">Melon</li>
  <li class = "banana">Banana</li>
</ul>
```
위의 코드에서 #fruits 요소는 3개의 자식 노드를 갖는데 먼저 ul#fruits 노드를 취득한다.

그 이후, 자식 노드를 탐색하는데 .melon 요소의 경우 2개의 형제 노드와 1개의 부모 노드를 갖는 것을 알 수 있다. 

따라서, 위에 말했듯이 주변의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

### 39.3.1 공백 텍스트 노드
텍스트 요소 사이의 줄바꿈에서 생겨나는 공백은 텍스트 노드를 생성하며 이를 공백 텍스트 노드라 한다.

그러나 이를 제거하게 된다면 가독성이 좋지 않게 변하므로 왠만하면 공백 텍스트 노드를 유지한다.

### 39.3.2 자식 노드 탐색

Node.prototype.chilNodes : 자식 노드를 모두 탐색하여 NodeList에 담아 반환 이때 NodeList에는 요소 노드 외에도 텍스트 노드가 포함될 수도 있다.

Element.prototype.children : 자식 노드 중에서도 요소 노드만 모드 탐색하여 HTMLCollection에 담아 반환한다. 이때, 텍스트 노드가 포함되지 않는다.

Node.prototype.firstChild : 자식 노드 중 첫 번째 자식 노드를 반환한다. 이때, 텍스트 노드이거나 요소 노드이다.

Node.prototype.lastChild : 자식 노드 중 마지막 자식 노드를 반환한다. 이때, 텍스트 노드이거나 요소 노드이다.

Element.prototype.firstElementChild : 자식 노드 중 첫 번째 자식 노드를 반환한다. 이때, 요소 노드이다.

Element.prototype.lastElementChild : 자식 노드 중 마지막 자식 노드를 반환한다. 이때, 요소 노드이다.

### 39.3.3 자식 노드 존재 확인
자식 노드가 존재하는지 확인하기위해서는 Node.prototype.hasChildNdoes 메서드를 사용한다. 

이 메서드는 텍스트 메서드를 포함하여 존재가 확인된다.

만약 자식 노드 중 텍스트 노드가 아닌 요소 노드의 존재를 확인하려면 children.length 프로퍼티를 사용한다. 

### 39.3. 4 요소 노드의 텍스트 노드 탐색
요소 노드에서의 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 통해 접근할 수 있다.

### 39.3.5 부모 노드 탐색
부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다. 

텍스트 노드는 위에서 말했듯이 리프 노드이기 때문에 부모 노드는 텍스트 노드가 됢 수 없다.

### 39.3.6 형제 노드 탐색
형제 노드 탐색은 자식 노드 탐색과 매우 유사하다.

Node.prototype.previousSibling 프로퍼티는 자신의 이전 형제 노드를 탐색하는 것이다.

Node.prototype.nextsSibling 프로퍼티는 자신의 다음 형제 노드를 탐색하는 것이다.

Element.prototype.previousElementSibling 프로퍼티는 자신의 이전 형제 노드를 탐색하지만 요소 노드만 반환 가능하다.

Element.prototype.nextElementSibling 프로퍼티는 자신의 다음 형제 노드를 탐색하지만 요소 노드만 반환 가능하다.

## 39. 4 노드 정보 취득
정보 취득을 위해서는 다음의 정보 프로퍼티를 사용해야한다.
Node.prototype.nodeType : 노드의 종류를 반환한다. 즉, 노드의 타입을 나타내는 상수를 반환

Node.prototype.nodeNmae : 노드의 이름을 문자열로 반환. 요소 노드의 경우 태그 이름을 반환하거나 문서 노드의 경우 "#document"를 반환

## 39.5 요소 노드의 텍스트 조작
### 39.5.1 nodeValue
Node.prototype.nodeValue 프로퍼티는 참조와 할당이 가능한 접근자 프로퍼티이다.

이 프로퍼티를 사용한다면 노드 객체의 값인 텍스트 노드의 텍스트를 반환하게 된다.

만약 텍스트 노드가 아닐 경우에는 null 값을 반환한다. 

그래서 텍스트를 변경하기 위한 순서는 아래와 같다.

1. 텍스트 노드의 부모 노드인 요소 노드를 취득 후, firstChild프로퍼티를 사용해 텍스트 노드를 탐색한다.

2. 탐색한 텍스트노드를 nodeValue 프로퍼티를 사용해 값을 변경한다.

### 39.5.2 textContent
textContent 프로퍼티를 사용한다면 시작 태그와 종료 태그내의 텍스트를 반환할 수 있다.
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <div id = 'foo'>hello <span>world!</span></div>
    </ul>
    <script>
        console.log(documnet.getElementById('foo').textContent); // hello world!
        </script>
</body>
</html>
```
또한, 위의 코드에서 textContent 프로퍼티는 foo영역에서 모든 텍스트를 반환하기 때문에 nodeValue 프로퍼티보다 코드가 덜 복잡하다.

유사한 프로퍼티는 innerText 프로퍼티가 있으나 CSS에 종속적이고 CSS가 고려대상이기 때문에 속도가 느려 쓰이지는 않는다.
