### 26. ES6 함수의 추가 기능

26.1 함수의 구분

**ES6** 이전까지 자바스크립트의 함수는 별다른 구분 없이 다양한 목적으로 사용되었다. 자바스크립트의 함수는 일반적인 함수로서 호출할 수도 있고，**new** 연산자와 함께 호출하여 인스턴스를 생성할 수 있는 생성자 함수로서 호출할 수도 있으며，객체에 바인딩되어 메서드로서 호출할 수도 있다. 이는 언뜻 보면 편리한 것 같지만 실수를 유발시킬 수 있으며 성능 면에서도 손해다.

```jsx
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

따라서 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출 할수도 있다

객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 흔치는 않겠지만 문법상 가능하다는 것은 문제가 있다. 그리고 이는 성능 면에서도 문제가 있다. 객체에 바인딩된 함수가 constructor 라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며，프로토타입 객체도 생성한다는 것을 의미하기 때문이다.

함수에 전달되어 보조 함수의 역할을 수행하는 콜백 함수도 마찬가지다. 콜백 함수도 constructor 이기 때문에 불필요한 프로토타입 객체를 생성한다

이러한 문제를 해결하기 위해 함수를 사용 목적에 따라 세 가지 종류로 구분한다.

(471쪽의 표 참조)

## 26.2 매서드

**ES6 사양에서 메서 드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**

```jsx
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다

ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

```jsx
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```

ES6 메서드가 아닌 함수는 **super** 키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯

**[[HomeObject]]**를 갖지 않기 때문이다.

이처럼 ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다. **따라서 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.**

## 26.3 화살표 함수

화살표 함수는 표현만 간략한 것이 아니라 내부동작도 기존의 함수보다 간략하다.

특히 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

**화살표 함수와 일반 함수의 차이**

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다. 화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다
2.  중복된 매개변수 이름을 선언할 수 없다.
3. 화살표함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다

## 26.4 this 함수

함수를 호출할 때 함수가 어떻게 호출되 었는지에 따라 **this**에 바인딩할 객체가 동적으로 결정된다.

```jsx
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    // ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

**Array.prototype.map**의 인수로 전달한 콜백 함수의 내부인 2에서 **this**는 **undefined**를 가리킨다.

이는 **Array.prototype.map** 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다.

**“this”**에서 살펴보았듯이 일반 함수로서 호출되는 모든 함수 내부의 **this**는 전역 객체를 가리킨다. 

그런데 클래스 내부의 모든 코드에는 strict mode가 암묵적으로 적용된다.  **Array.prototype.map** 메서드의 콜백 함수에도 strict mode가 적용된다. strict mode에서 일반함수로서 호출된 모든 함수 내부의 **this**에는 전역 객체가 아니라 **undefined**가 바인딩되므로 일반 함수로서 호출되는 **Array.prototype.map** 메서드의 콜백 함수 내부의 **this**에는 **undefined**가 바인딩된다.

이때 발생하는 문제가 바로 **콜백함수 내부의 this 문제**다.

```jsx
...
add(arr) {
  // this를 일단 회피시킨다.
  const that = this;
  return arr.map(function (item) {
    // this 대신 that을 참조한다.
    return that.prefix + ' ' + item;
  });
}
...
//add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.
```

```jsx
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
//Array.prototype.map의 두 번째 인수로
//add메서드를 호줄한 prefixer객체를 가리키는 this롤 전달한다.
...
```

```jsx
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
//bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩 한다
```

## 26.5 매개변수 기본값

매개 변수 기본값은 매개변수에 **인수를 전달하지 않은 경우**와 **undefined를 전달한 경우**에만 유효하다.
