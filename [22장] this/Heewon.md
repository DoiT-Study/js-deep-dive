# 22. this

## 22.1. this 키워드

메서드는 자신이 속한 객체의 프로퍼티를 참조할 수 있어야 한다. 이를 위해서는 해당 객체를 가리키는 식별자를 참조할 수 있어야 한다. 다음의 예시를 들 수 있다.

```jsx
const circle = {
    radius: 5;

    getdiameter() {
        // circle 객체의 식별자 참조
        return 2 * circle.radius;
    }
}
```

내부 실행 순서는 다음과 같다.

객체 리터럴 평가 -> 객체 생성 -> `circle` 식별자에 객체 할당 (`circle` 사용 가능) -> `getDiameter` 메서드 호출

따라서 메서드 내부에서 `circle` 식별자를 참조할 수 있다. 그러나 이러한 방식은 권장되지 않는다. 생성자 함수 방식으로 인스턴스를 생성할 경우 인스턴스 생성 이전에 생성자 함수가 정의되므로 함수 내부에서 인스턴스 식별자를 알 수 없다.

이 문제를 해결하기 위해 `this` 라는 특수한 식별자를 사용한다. 

> `this` : 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수

- 자바스크립트 엔진에 의해 암묵적으로 생성된다.
- 코드 어디서든 참조할 수 있다. 
    - 전역에서 참조할 시 전역 객체 `window`를 가리킨다.
    - strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.
- 지역 변수처럼 사용할 수 있다.
-  `this`가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.

```jsx
const circle = {
    radius: 5;

    getdiameter() {
        // this: 메서드를 호출한 객체 (circle)
        return 2 * this.radius;
    }
}
```

```jsx
function Circle(radius) {
    // this: 생성자 함수가 생성할 인스턴스
    this.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // this: 생성자 함수가 생성할 인스턴스
    return 2 * this.radius
}

const circle = new Circle(5); // function Circle의 this: circle

console.log(circle.getDiameter()); // 10
```

```jsx
console.log(this); // window

function square(number) {
    // 일반 함수 내부
    console.log(this); // window
    return number * number;
}
```

## 22.2. 함수 호출 방식과 this 바인딩

> this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

### 22.2.1. 일반 함수 호출

전역 또는 일반 함수 내부에서 `this`는 전역 객체 `window`를 가리킨다. 그러나 strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.

```jsx
function foo() {
    console.log("foo's this: ", this) // foo's this: window
    function bar() {
        console.log("bar's this: ", this) // bar's this: window
    }
    bar();
}
foo();
```

```jsx
function foo() {
    'use strict';

    console.log("foo's this: this"); // undefined
    function bar() {
        console.log("bar's this: ", this); // undefined
    }
    bar();
}
foo();
```
<br>

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 `this`에 전역 객체가 바인딩된다.

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // foo's this: {value: 100, foo: f}
        console.log("foo's this.value: ", this.value); // 100

        function bar() {
            console.log("bar's this: ", this); // bar's this: window
            console.log("bar's this.value:", this.value); // 1
        }

        bar();
    }
};

obj.foo();
```

<br>

콜백 함수 또한 일반 함수로 호출되면 콜백 함수 내부의 `this`에 전역 객체가 바인딩된다.

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // foo's this: {value: 100, foo: f}
        setTimeout(function () {
            console.log("callback's this: ", this); // callback's this: window
            console.log("callback's this.value: "); // 1
        }, 100);
    }
};

obj.foo();
```

<br>

> 이처럼 **일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함)** 내부의 `this`에는 전역 객체가 바인딩된다.

그러나 중접 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 하므로 외부 함수의 일부 로직을 대신할 때가 많다. 외부 함수(메서드)와 중접/콜백 함수의 `this`가 일치하지 않는다는 것을 중첩/콜백 함수를 헬퍼 함수로 동작하기 어렵게 한다. 이를 일치시키는 방법에는 세 가지가 있다.

1. `that` 변수를 만들어 `this`를 할당한다.

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        const that = this;

        setTimeout(function () {
            console.log(that.value); // 100
        }, 100);
    }
};

obj.foo();
```

2. `this`를 명시적으로 바인딩할 수 있는 `Function.prototype.apply`, `Function.prototype.call`, `Function.proptotype.bind` 메서드를 사용한다.

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        setTimeout(function () {
            console.log(this.value); // 100
        }.bind(this), 100);
    }
};

obj.foo();
```

3. 화살표 함수를 사용하여 `this` 바인딩을 일치시킨다.

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        setTimeout(() => console.log(that.value), 100); // 100
    }
};

obj.foo();
```

### 22.2.2. 메서드 호출

메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

```jsx
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function () {
    return this.name;
}

const me = new Person('Lee');

// getName 메소드를 호출한 객체: me
console.log(me.getName()); // Lee

Person.prototype.name = 'Kim';

// getName 메소드를 호출한 객체: Person.prototype
console.log(Person.prototype.getName()); // Kim
```

### 22.2.3. 생성자 함수 호출

생성자 함수 내부의 `this`에는 생성자 함수가 추후에 생성할 인스턴스가 바인딩된다.

```jsx
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

만약 `new` 연산자로 인스턴스를 만들어 생성자 함수를 호출하는 것이 아니면 생성자 함수가 아닌 일반 함수로 동작한다.

```jsx
const circle3 = Circle(15);

console.log(circle3); // undefined (Circle에 반환문 없음)

console.log(radius); // 15 (this: 전역객체, 이 부분 이해 못함)
```

### 22.2.4. `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

`apply`, `call`, `bind` 메서드는 `Function.prototype`의 메서드로, 모든 함수가 상속받아 사용할 수 있다. 

- `apply` : 주어진 `this` 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
- `call` : 주어진 `this` 바인딩과 `,`로 구분된 인수 리스트를 사용하여 함수를 호출한다.

이들의 대표적인 용도는 `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 것이다. 다음 예제는 `apply`와 `call` 메서드를 통해 `getThisBinding` 함수를 호출하며 인수를 전달한다.

```jsx
function getThisBinding() {
    console.log(arguments);
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1};

// 호출할 함수의 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));

// 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
console.log(getThisBinding.call(thisArg, 1, 2, 3));
```

- `bind` : 함수를 호출하지 않는다. `this` 바인딩을 **첫 번째 인수로 전달한 값**으로  교체한 함수를 새롭게 생성해 반환한다. 

메서드의 `this`와 메서드 내부의 중첩/콜백 함수의 `this`가 불일치하는 문제를 해결할 때 유용하게 사용된다.

```jsx
const person = {
    name: 'Lee', 
    foo(callback) {
        setTimeout(callback, 100);
    }
};

person.foo(function () {
    // 콜백 함수가 일반 함수로서 호출되었기에 this는 window이다.
    console.log(`Hi! My name is ${this.name}.`);
})
```

```jsx
const person = {
    name: 'Lee', 
    foo(callback) {
        // 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치시킨다.
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function () {
    console.log(`Hi! My name is ${this.name}.`); // Hi! My name is Lee.
})
```
