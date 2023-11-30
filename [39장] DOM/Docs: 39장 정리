39.5

Node.prototype.nodeValue 프로퍼티 : setter와 getter 모두 존재하는 접근자 프로퍼티. 즉 참조와 할당 모두 가능하다.

### textContent

Node.prototype.textContext 프로퍼티 : setter와 getter 모두 존재한다.

요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환. HTML 마크업은 무시된다.

textContext와 유사한 동작을 하는 innerText 프로퍼티가 있음. 사용하지 않는 것이 좋다.

- innerText 프로퍼티는 CSS에 순종적. 예를 들어 innerText 프로퍼티는 CSS에 의해 비표시(visibility : hidden)로 지정된 요소 노드의 텍스트를 반환하지 않는다.
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContext보다 느리다.

## DOM 조작

### innerHTML

Element.prototype.innerHTML 프로퍼티 : getter와 setter 모두 존재하는 접근자 프로퍼티.

textContent 프로퍼티를 참조하면 HTML마크업을 무시하고 텍스트만 반환하지만 innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환한다.

사용자로부터 입력받은 데이터(untrusted input data)를 그대로 innerHTML 프로퍼티에 할당하는 것은 **크로스 사이트 스크립팅 공격(XSS : Cross-Site Scripting Attacks)**에 취약하므로 위험함.

HTML 마크업 내에 자바스크립트 악성 코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있기 때문이다.

HTML5는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않지만, script 없이도 사이트 스크립트 공격은 가능하다.

innerHTML은 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.

**insertAdjacentHTML**

기존 요소에 영향 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 innerHTML 프로퍼티보다 효율적이고 빠름. 단, innerHTML 프로퍼티와 마찬가지로 HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립칭 공격에 취약하다는 점은 동일하다.

### 노드 생성과 추가

- Document.prototype.createElement(tagName) - 요소 노드 생성
- Document.prototype.createTextNode(text) - 텍스트 노드 생성
- Node.prototype.appendChild(childNode) - 텍스트 노드르 자식의 노드로 추가

• Node.prototype.appendChild - 요소 노드를 DOM에 추가

### 노드 삽입

Node.prototype.appendChild

node.protoype.insertBefore(newNode, childNode) - 지정한 위치에 노드 삽입

두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드

### 노드 이동

appendChild, insertBefore 메서드 사용한다.

### 노드 복사

Node.prototype.cloneNode([deep : true | false]) 메서드는 노드의 사본을 생성하여 반환한다.

### 노드 교체

Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체함.

### 노드 삭제

Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제.

인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야한다.
