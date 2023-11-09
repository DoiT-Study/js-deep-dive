# 22장 this
## 22.1 this 키워드

동작을 나타내는 메서드는 프로퍼티를 참조하여야 하는데 이때, 객체를 가리키는 식별자를 먼저 참조해야한다.

```javascript
const {
radius: 3;
getDiameter () { // getDiameter 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면
                 // 자신이 속한 객체 circle을 참조할 수 있어야한다.
return 22 * circle.radius;
}
}
```

```javascript
function circle(radius) {
????.radius = radius; // 여기서 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
}

circle.prototype.getdiameter = function () {
return 2 * ???.radius; // 이 경우 또한 인스턴스를 가리키는 식별자를 알 수 없다.
};

const circle = new circle(5); // 그래서 인스턴스를 먼저 생성하려면 생성자 함수를 정의해야한다.
```
여기서 생성자 함수를 정의하는 시점은 인스턴스를 생성하기 전이기 때문에 인스턴스가 가리키는 식별자를 알 수 없다.

그래서 자바스크립트는 this라는 식별자를 사용한다.

this는 자신의 객체나 생성할 인스턴스를 가리키는 자기 참조 변수이다.

또한, this가 가리키는 값는 함수 호출 방식을 따른다.

```javascript
const circle = {
radius: 5;
getdiameter() {
return 2 * this.radius; // 여기서 this는 메서드를 호출한 자신의 객체 circle를 가리킨다.
}
};

console.log(circle.getdiameter()); // 10;
```

```javascript
function circle(radius) {
this.radius = radius; // 여기서 this는 생성자 함수의 인스턴스를 가리킨다.
}

circle.prototype.getdiameter = function () {
return 2 * this.radius; // 이 경우 또한 this는 생성자 함수의 인스턴스를 가리킨다.
};

const circle = new circle(5); 
```

그러나 this 바인딩은(this와 this가 객체를 가리키는 과정) 동적으로 생성된다.

또한, 어디에서나 참조가 가능하고 함수 내부에서도 가능하다.

자기 참조 변수이므로 생성자 함수 내부, 객체 메서드 내부에서 효과적으로 작용하지만 일반 함수에서는 사용될 필요가 없다. 

## 22.2 함수 호출 방식과 this 바인딩

위에 말했듯이, this 바인딩은 동적으로 생성되는데 함수가 어떻게 호출되었는지에 따라 다르다.

일반 함수 호출, 메서드 호출, 생성자함수 호출, 간접 호출과 같은 다양한 방식이 있다.

### 일반 함수 호출
우선 일반 함수 호출의 경우, this에는 전역 객체가 바인딩된다.

```javascript
function foo () {
console.log("foo's this: ", this); // window
}
foo();
```
위 코드의 경우, 함수 내부의 this는 전역 객체 window가 바인딩 되는 것을 볼 수 있다.

이외에도 중첩 함수를 일반 함수로 호출한다면 중첩 함수 내부의 this에는 전역 개체가 바인딩된다.

만약 strict mode를 사용한다면 window가 아닌 undefined가 바인딩된다.

setTimeout과 같은 콜백 함수가 일반 함수로 호출될 경우 콜백 함수 내부의 this는 전역 개체 window를 바인딩한다.

즉, 일반 함수로 호출된 모든 함수 내부에 this가 있다면 전역 개체를 가리킨다.

그러나 콜백 함수, 중첩 함수, 콜백 함수의 this가 외부 함수와 일치하지 않는다는 것은 중첩, 콜백 함수의 헬퍼 역할을 동작하기 어렵게 만든다.

```javascript
var value = 1;
const obj = {
value: 100;
foo() {
console.log("foo's this : ", this);
setTimeout(function() {
console.log("callback's this : ", this); // window
console.log("callback's this.value : ", this.value); // 1
}, 100);
}
};
```
위 코드의 경우, setTimeout 함수에서 콜백 함수의 this는 window를 바인딩한다.

그러나 this.value는 obj 객체의 value가 아닌 window의 value를 참조하므로 window.value는 100이 아닌 1이 된다.

즉, 객체가 일치하지 않는다.

이를 해결하기 위해서 obj 객체를 변수 that으로 따로 설정하고 this대신 that을 참조하면 콜백 함수의 this와 메서드의 this 바인딩을 일치시킬 수 있다.

###  메서드 호출
메서드는 객체에 포함된것이 아닌 독립적으로 존재하는 객체이다.

따라서, 메서드 내부의 this는 메서드를 가지는 객체가 아닌 호출한 객체에 바인딩된다.

```javascript
const person = {
name : 'shin',
getName() {
return this.name;
}
};
console.log(person.getName()); // shin
                               // getName 메서드를 호출한 객체는 person 이므로 this는 person에 바인딩된다.
```
프로토타입 매서드 내부에서 사용된 this 또한, 메서드를 호출한 객체에 바인딩된다.

### 생성자 함수 호출

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스에 바인딩된다.

생성자 함수는 객체를 생성하는 함수로 생성자 함수를 정의하고 new연산자로 호출한다면 생성자 함수로 작동한다.

new가 없다면 일반 함수가 될 것이다.

```javascript
function Circle(radius) { //생성자 함수
this.radius= radius; // 이 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
this.getdiameter = function() {
return 2*this.radius;
};
}
const circle1 = new Circle(5);
console.log(circle1.getdiameter()); // 10
```
### Function.prototype.apply / call / bind 메서드에 의한 간접 호출

apply / call / bind는 Fucntion.prototype의 메서드이다. 따라서, 이들은 모든 함수가 상속받을 수 있다.

apply와 call은 this로 사용할 객체를 인수로 전달받아 함수를 호출하는 방식이다.

이 둘의 차이점은 인수 리스트의 표현 방식이다.

apply 메서드는 인수를 배열로 묶어 전달하지만 call 메서드는 인수를 ,로 리스트로 전달한다. 

그러나 bind 메서드는 함수를 호출하지 않고 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 반환한다.
```javascript
function getthisbinding() {
return this;
}
const thisarg = {a:1};
//thisarg를 첫 번째 인수로 전달받아 this 바인딩이 교체된 getthisbindding 함수를 새롭게 생성한다.
console.log(getthisbinding.bind(thisarg)); // getthisbinding
console.log(getthisbinding.bind(thisarg)()); // {a:1}
```
또한, 메서드의 this와 메서드 내부의 중첩, 콜백 함수의 this가 불일치하는 문제를 해결한다.

