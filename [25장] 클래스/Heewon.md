# 25. 클래스

## 25.1. 클래스는 프로토타입의 문법적 설탕인가?

자바스크립트는 프로토타입 기반 객체지향 언어다. 프로토타입 기반 객체지향 언어는 클래스를 필요로 하지 않는다. 그러나 클래스 기반 언어에 익숙한 프로그래머들이 프로토타입 기반 프로그래밍 방식에 혼란을 느끼는 문제가 발생하였다. 이를 해결하기 위해 ES6에서 클래스가 도입되었다.

클래스는 생성자 함수와 유사하게 동작하지만 다음과 같은 차이점이 존재한다.

||클래스|생성자 함수|
|:------:|:--:|:---:|
|`new` 연산자 없이 호출할 경우|에러 발생|일반 함수로서 호출|
|상속 키워드 `extends`, `super`|지원함|지원하지 않음|
|호이스팅|일어나지 않는 것처럼 동작|함수/변수 호이스팅 발생|
|`strict mode`|암묵적으로 실행되며 해제 불가|암묵적으로 지정되지 않음|
|`constructor`, 프로토타입/정적 메서드|`[[Enumerable]]`=`false` (열거X)|변경 가능|

클래스를 단순히 프로토타입의 문법적 설탕으로 보기보다는 **새로운 객체 생성 메커니즘**으로 보는 것이 적절하다.

## 25.2. 클래스 정의

클래스는 `class` 키워드를 사용하여 정의한다. 파스칼 케이스를 사용하여 명명하는 것이 일반적이다. 또한 클래스는 자바스크립트에서 일종의 함수이며, 따라서 일급 객체이다.

```jsx
class Person {}

/* 표현식으로 정의 */
const Person = class {}

const Person = class MyClass {}
```

클래스 몸체에는 `constructor(생성자)`, 프로토타입 메서드, 정적 메서드를 정의할 수 있으며, 아무것도 정의하지 않아도 괜찮다.

```jsx
class Person {
    constructor(name) [
        this.name = name;
    ]

    /* Prototype method */
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }

    /* Static method */
    static sayHello() {
        console.log('Hello!');
    }
}

const me = new Person('Lee')

/*프로토타입 메서드 호출*/
me.sayHi(); // Hi! My name is Lee

/* 정적 메서드 호출 */
Person.sayHello(); // Hello!
```

클래스와 생성자 함수의 정의 방식을 비교하면 다음과 같다.

||생성자 함수|클래스|
|:------:|:--:|:---:|
|생성자 (함수)|`function Person(name)`|`constructor(name)`|
|프로토타입 메서드|`Person.prototype.sayHi = function () {}`|`sayHi() {}`|
|정적 메서드|`Person.sayHello = function () {}`|`static sayHello() {}`|
|반환|`return Person`|없음|

## 25.3. 클래스 호이스팅

클래스 선언문은 `let`, `const` 선언문처럼 마치 호이스팅이 발생하지 않는 것처럼 동작한다. 그러나 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다. 값이 할당되기 전에 일시적 사각지대에 빠지는 것 뿐이다.ㅓ

```jsx
const Person = '';

{
    console.log(Person); 
    // ReferenceError: Cannot access 'Person' before initialization
    // 호이스팅이 발생한다는 것을 보여준다

    class Person {}
}
```

## 25.4. 인스턴스 생성

클래스는 `new` 연산자와 함께 호출되어 인스턴스를 생성한다. `new` 연산자를 사용하지 않을 경우 에러가 발생한다.

```jsx
class Person {}

const me = new Person();
const you = Person(); 
// TypeError: Class constructor Person cannot be invoked without 'new'
```

클래스 표현식으로 정의된 클래스의 경우 식별자를 사용해 인스턴스를 생성해야 한다. 클래스 이름을 사용해 인스턴스를 생성하면 에러가 발생한다.

```jsx
const Person = class MyClass {};

const me = new Person();
const you = new MyClass(); // ReferenceError: MyClass is not defined
```

## 25.5. 메서드

클래스 몸체에서 정의할 수 있는 메서드는 `constructor(생성자)`, 프로토타입 메서드, 정적 메서드 세 가지가 있다.

### 25.5.1. `constructor`

`constructor`는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드이다. 그러나 개발자 도구에서 객체 및 인스턴스를 살펴보면 `constructor` 메서드가 보이지 않는다. 클래스 몸체에 정의한 `constructor`가 단순한 메서드가 아니기 때문이다. `constructor`는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.

`constructor`가 생성자 함수와 다른 점은 다음과 같다.

- **클래스 내에 단 한 개만 존재할 수 있다.** 두 개 이상일 시 `SyntaxError`가 발생한다.
- **생략할 수 있다.** 프로퍼티가 추가되어 초기화된 인스턴드를 생성할 때는 `constructor` 내부에 `this.property`를 추가한다.
- **별도의 반환문을 갖지 않아야 한다.** 클래스는 암묵적으로 `this`를 반환하는데 이때 다른 객체를 명시적으로 반환하면 클래스의 기본 동작을 훼손한다.

### 25.5.2. 프로토타입 메서드

클래스 몸체에서 정의한 프로토타입 메서드는 `prototype` 프로퍼티에 메서드를 추가하지 않아도 된다. 자동으로 프로토타입 체인의 일원에 속한다.

### 25.5.3. 정적 메서드

정적 메서드란, 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다. 클래스에서는 메서드에 `static` 키워드를 붙이면 정적 메서드 (클래스 메서드) 가 된다. 정적 메서드는 클래스 자체에 바인딩되기에 인스턴스로 호출하지 않고 클래스로 호출한다. 애초에 인스턴스로 호출할 수 없다. 인스턴스의 프로토타입 체인에 해당 메서드가 존재하지 않기 때문이다.

```jsx
const me = new Person('Lee');
me.sayHi(); // TypeError: me.sayHi is not a function
```

### 25.5.4. 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

메서드 내부에서 인스턴스 프로퍼티를 참조할 일이 있다면 프로토타입 메서드로, 참조할 필요가 없다면 정적 메서드로 정의하는 것이 좋다.

다음은 같은 동작을 하는 메서드를 정적/프로토타입으로 각각 구현한 예제이다.

```jsx
class Square {
    static area(width, height) {
        return width * height;
    }
}

console.log(Square.area(10, 10)); // 100
```

```jsx
class Square {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
}

const square = new Square(10, 10);
console.log(Square.area()); // 100
```

### 25.5.5. 클래스에서 정의한 메서드의 특징

1. `function` 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 `strict mode`로 실행된다.
4. `for ... in` 문이나 `Object.keys` 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `false`다.
5. 내부 메서드 `[[Construct]]`를 갖지 않는 non-constructor이다. 따라서 `new` 연산자와 함께 호출할 수 없다.

## 25.6. 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 `this` 바인딩

    `constructor`의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성된다.

2. 인스턴스 초기화

    `constructor`의 내부 코드가 실행되어 인스턴스에 프로퍼티를 추가하고 초기화한다.

3. 인스턴스 반환

    `this`가 암묵적으로 반환된다.

<br>

# 느낀 점 & 토론

기본적으로 자바나 C++를 공부하며 클래스의 기본 개념은 알고 있기에 보다 수월하게 읽고 이해해낼 수 있었다. 지금까지 생성자 함수를 이용한 객체 생성이 직관적으로 와닿지 않아 큰 어려움을 겪었는데 클래스 개념이 도입돼 있어 다행이었다. 다만 클래스의 도입이 자바스크립트라는 언어의 본질을 흐리는 것이 아닌가, 라는 생각이 든다. 

> 자유도가 높고 프로토타입과 함수 객체를 기본 구성으로 하는 것이 자바스크립트인데 이를 지나치게 제한해버려 자바스크립트만의 차별성을 잃는 것인지 vs 동작을 제한하는 클래스조차도 자바스크립트의 자유도에 포함되어 사용자 맞춤형으로 계속해서 업데이트되는 특성에 걸맞는 것인지

이 두 견해를 생각해 봤는데 아직 결론을 내리지 못했다. 다 같이 이야기해 봐도 좋을 것 같다.