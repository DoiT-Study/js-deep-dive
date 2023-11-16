# 19 프로토 타입

## 19.7 프로토타입 체인

> Object.prototype은 프로토타입 체인의 종점이다.

프로토 타입 체인이란?

자바스크립트에서는 객체가 특정 기능을 필요로 할때, 먼저 자신의 내부에서 해당 기능을 찾는다. 그런데 없다면 부모와 같은 존재인 프로토 타입에서 이 기능을 찾게된다. 이와 같이 상위 프로토 타입을 따라가며 기능을 찾는 메커니즘을 프로토 타입 체인이라고 한다.

```js
const person = { name: "LEE", adress: `Seoul` };

function Person(name) {
  this.name = name;
}

//프로토 타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person(`Lee`);

// hasOwnProperty는 object.property의 메서드다
console.log(me.hasOwnProperty(`name`));
```

```
true
```

### 설명

우리가 Person이라는 함수로 'me'라는 객체를 만들었다고 해보자. 'me'는 이름을 가지고 있고, hasOwnProperty라는 메소드로 내 속성인지 확인하려고 한다. 그런데 이 메소드는 'me'에게는 없다. 그래서 'me'의 프로토타입인 Person.prototype을 보고, 여기에도 없으면 그 위인 Object.prototype을 본다. hasOwnProperty는 Object.prototype의 메소드니 결국 여기에서 이 메소드를 찾게 된다.

### 객체의 공통 조상 Object.prototype

이 Object.prototype은 프로토타입 체인의 맨 꼭대기에 있다. 여기에서도 찾지 못하는 속성을 요청하면, 자바스크립트는 undefined를 돌려준다.

### 프로토타입 체인과 스코프 체인의 차이점

프로토타입 체인은 객체의 속성을 찾는 과정을 말하고, 스코프 체인은 함수 안에서 변수를 찾는 과정을 말한다. 둘은 각자의 방식으로 우리가 필요한 값을 찾도록 도와준다.

## 19.8 오버라이딩과 프로퍼티 섀도잉

'me'라는 객체에 새로운 기능을 추가하거나 기존의 기능을 바꾸는 것을, 이를 '오버라이딩'이라고 한다. 예를 들어 'sayHello'라는 기능이 있었는데, 'me'만의 특별한 인사를 하고 싶어서 이 기능을 새로 써넣었다고 생각해 보자. 이제 'me'는 새로운 방식으로 인사를 하게 되고, 원래 프로토타입에 있던 'sayHello'는 가려지게 된다. 이것이 바로 '프로퍼티 섀도잉'이다.

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  //프로토 타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  return Person;
})();

const me = new Person(`Lee`);

me.sayHello = function () {
  console.log(`hey! My name is ${this.name}`);
};

me.sayHello();
```

```
hey! My name is Lee
```

## 19.9 프로토 타입의 교체

프로토타입을 바꾸는 것은 가능하지만, 일반적으로는 추천되지 않는다.
왜냐하면 복잡하고 예상치 못한 문제를 일으킬 수 있기 때문이다.
ES6의 클래스를 사용하거나 다른 방법으로 상속을 구현하는 것이 더 좋은 방법이 될 수있을 것이다.

### 생성자 함수에 의한 프로토 타입의 교체

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  Person.prototype = {
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("WI");
console.log(me.constructor === Person);
console.log(me.constructor === Object);
```

```
false
true
```

### 인스턴스 함수에 의한 프로토 타입의 교체

```js
function Person(name) {
  this.name = name;
}

const me = new Person("WI");

const parent = {
  sayHello() {
    console.log(`Hi, My name is ${this.name}`);
  },
};

Object.setPrototypeOf(me, parent);
console.log(me.constructor === Person);
console.log(me.constructor === Object);
```

```
false
true
```

## 19.10 instanceof 연산자

> instanceof 연산자를 사용하면 객체가 특정 생성자의 인스턴스인지 확인할 수 있다. 이는 객체의 프로토타입 체인을 거슬러 올라가며 생성자의 prototype 속성이 체인 상에 있는지를 확인하는 방식으로 작동한다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("WI");

// 프로토타입으로 교체할 객체
const parent = {};

Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent);
console.log(parent.constructor === Person);

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩
Person.prototype = parent;

// Person.prototype 이 me 객체의 프로토타입 체인 상에 존재함
console.log(me instanceof Person);
// Object.prototype 이 me 객체의 프로토타입 체인 상에 존재함
console.log(me instanceof Object);
```

```
false
false
true
true
```

me instanceof Person이 참인 이유는 Person.prototype이 me의 프로토타입 체인에 있기 때문이고 me instanceof Object 역시 참인 이유는 모든 객체가 궁극적으로 Object.prototype을 상속받기 때문이다.

## 19.11 직접상속

### Object.create를 이용한 직접상속

Object.create 메소드를 사용하면 명시적으로 프로토타입을 설정하여 새 객체를 생성할 수 있다. 이 방법을 이용하여 생성자 함수 없이도 객체를 생성할 수 있으며, 원하는 프로토타입 체인을 직접 구성할 수 도 있게 해준다.

```js
Object.creat(prototype[, properties,Object]);
```

```js
// obj -> null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null);

// obj -> Object.prototype -> null
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype);

// obj -> Objrct.prototype -> null
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
console.log(Object.getPrototypeOf(obj) === Object.prototype);

const myProto = { x: 10 };
// obj -> myProto -> Object.prototype -> null
obj = Object.create(myProto);
console.log(Object.getPrototypeOf(obj) === myProto);

function Person(name) {
  this.name = name;
}
// obj -> Perosn.prototype -> Object.prototype -> null
obj = Object.create(Person.prototype);
console.log(Object.getPrototypeOf(obj) === Person.prototype);
```

```
true
true
true
true
true
```

### 객체 리터럴 내부에서 proto 에 의한 직접 상속

> Object.create 로 객체를 생성할 때, 두 번째 파라미터로 프로퍼티를 정의하는 것에 번거로움을 해소하는 방법이다

그러나 역시 깔끔한 방법은 아니다.

```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서, 프로토타입을 지정하여 직접 상속을 구현할 수 있다.
const obj = {
  y: 20,
  __proto__: myProto,
};

console.log(obj.x, obj.y);
console.log(Object.getPrototypeOf(obj) === myProto);
```

```
10 20
true
```

## 19.12 정적 프로퍼티/메서드

> 생성자 함수는 객체이다.

정적 프로퍼티와 메서드는 생성자 함수 자체에 직접 부여되며, 인스턴스를 생성하지 않아도 사용할 수 있다.

이러한 정적 멤버들은 해당 생성자의 모든 인스턴스와 공유되지 않고, 생성자 함수 자체에만 바인딩된다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`HI, My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "인간 생성자 함수의 정적 프로퍼티 !";

// 정적 메서드
Person.staticMethod = function () {
  console.log("인간 생성자 함수의 정적 메서드 호출 !");
};

const me = new Person("WI");

Person.staticMethod(); // 인간 생성자 함수의 정적 메서드 호출 !
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

## 19.13 프로퍼티 존재 확인

```
key in object;
```

프로퍼티 존재 확인을 위해서는 in 연산자와 hasOwnProperty 메서드를 사용할 수 있다.
in 연산자는 객체의 자체 프로퍼티 뿐만 아니라 프로토타입 체인을 따라 상속된 프로퍼티까지 확인하지만, hasOwnProperty 메서드는 해당 객체의 고유 프로퍼티에 대해서만 true를 반환한다.

### in 연산자

```js
key in object;

const person = {
  name: "WI",
  age: 100,
};

console.log("name" in person);
console.log("age" in person);
console.log("address" in person);

//person 객체의 프로토타입인 Object.prototype 에 toString 메서드가 존재하기 때문에 true를 반한한다.
console.log("toString" in person);
```

```
true
true
false
true
```

### Object.prototype.hasOwnProperty 메서드

```js
const person = {
  name: "WI",
  age: 100,
};

console.log(person.hasOwnProperty("name"));
console.log(person.hasOwnProperty("age"));

// 객체의 고유 프로퍼티일 때만 true 를 반환하는 Object.prototype.hasOwnProperty 메서드는 false 를 반환할 것이다.
console.log(person.hasOwnProperty("toString"));
```

```
true
true
false
```

## 19.14 프로퍼티 열거

```
프로퍼티를 열거할 때는 for...in 루프를 사용할 수 있지만, 이 루프는 객체의 모든 열거 가능한 프로퍼티를 순회하며, 프로토타입 체인 상의 열거 가능한 프로퍼티까지 포함함한다.

객체 자체의 프로퍼티만을 열거하고 싶다면 hasOwnProperty를 사용하여 체크할 수 있다.

또한 Object.keys(), Object.values(), Object.entries() 메서드를 사용하여 객체 자신의 프로퍼티만을 열거할 수도 있습니다.
```

### for - in 문

```
for (변수선언문 in 객체) { ... }
```

```js
const person = {
  name: "WI",
  age: 100,

  __proto__: {
    address: "Incheon",
  },
};

console.log("toString" in person); // true

for (const key in person) {
  console.log(`${key} : ${person[key]}`);
}
```

```
name : WI
age : 100
address : Incheon
```

```js
const person = {
  name: "WI",
  age: 100,

  __proto__: {
    address: "Incheon",
  },
};

for (const key in person) {
  // person 객체의 고유 프로퍼티일 경우에만 정보를 출력
  if (person.hasOwnProperty(key)) {
    console.log(`${key} : ${person[key]}`);
  }
}
```

```
name : WI
age : 100

```
