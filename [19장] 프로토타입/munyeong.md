# 19 프로토타입

## 19.1 객체지향 프로그래밍
- 추상화 : 프로그래밍에 필요한 속성만 간추려 내어 표현하는 것
- 프로퍼티 : 상태를 나타내는 데이터
- 메서드 : 상태 데이터를 조작할 수 있는 동작
<br>

## 19.2 상속과 프로토타입
상속
<br>
어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것
<br>
- 주로 불필요한 중복을 제거하기 위해 사용
<br>
- 프로토타입을 기반으로 구현
<br>

예제)
```javascript
function Circle(radius) {
    this.radius = radius;
}

Circle.prototype.getArea = function () {
    return Math.PI * this.radius ** 2;
}
```
getArea 메서드는 단 하나만 생성되어 프로토타입(Circle.prototype)의 메서드로 할당되어 있기 때문에 Circle 생성자 함수가 생성하는 모든 인스턴스는 해당 메서드를 상속받을 수 있음
<br>
radius 프로퍼티만 개별적으로 소유하고 내용이 같은 메서드는 상속을 통해 공유하여 중복을 줄임
<br>

## 19.3 프로토타입 객체
프로토타입 객체
<br>
- 객체 간 상속을 구현하기 위해 사용
<br>
- 프로토타입은 다른 객체에 공유 프로퍼티를 제공 -> 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 사용 가능


### 19.3.1 __proto__ 접근자 프로퍼티
접근자 프로퍼티 : 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티
<br>
<br>

__proto__ 접근자 프로퍼티
- 모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있음
- __proto__ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아닌 Object.prototype의 프로퍼티
- 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해 사용
- 모든 객체가 __proto__ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문에 사용을 지양함 => Object.getPrototypeOf, Object.setPrototypeOf 메서드 사용 권장

```javascript
const obj = {};
const parent = {x : 1};

obj.__proto__ = parent;

console.log(obj.x);  // 1
```


### 19.3.2 함수 객체의 prototype 프로퍼티
함수 객체의 prototype 프로퍼티
<br>
"생성자 함수"가 생성할 인스턴스의 프로토타입
<br>
- 생성자 함수로서 호출할 수 없는 함수(ex. 화살표 함수)는 프로토타입 프로퍼티를 소유하지 않음
<br>
- __proto__ 접근자 프로퍼티와 prototype 프로퍼티는 동일한 프로토타입을 가리킴

```javascript
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__);
```
<br>

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수
constructor 프로퍼티
<br>
prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴

```javascript
function Person(name) {
    this.name = name;
}

const me = new Person('Lee')

console.log(me.constructor === Person);  // true
```
<br>

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
리터럴 표기법에 의해 생성된 객체
<br>
- OrdinaryObjectCreate를 호출하여 생성됨
- Object 생성자 함수에 의해 생성된 객체가 아님
- 가상적인 생성자 함수를 가짐 
<br>


## 19.5 프로토타입의 생성 시점
프로토타입 생성 시점 : 생성자 함수가 생성되는 시점

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점
- 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성됨
<br>


## 19.6 객체 생성 방식과 프로토타입의 결정
객체 생성 방식
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스
<br>

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입
객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype

```javascript
const obj = {x : 1};

console.log(obj.constructor === Object); // true
```
<br>

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입
Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype

```javascript
const obj = new Object();
obj.x = 1;

console.log(obj.constructor === Object);  // true
```
<br>

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입
생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function() {
    console.log('Hi I'm ${this.name}');
}

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi I'm Lee
you.sayHello();  //Hi I'm Kim
```

<br>



## 19.7 프로토타입 체인
프로토타입 체인
<br>
자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘
<br>
객체의 프로퍼티에 접근하려고 할 때 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색
<br>
- 프로토타입 체인의 종점(최상위)에는 Object.prototype이 존재
- Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환함
