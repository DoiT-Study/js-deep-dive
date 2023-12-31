# 26장 : ES6 함수의 추가 기능

# 26.1 함수의 구분

- ES6 이전까지 자바스크립트의 함수는 별다른 구분(분류) 없이 다양한 목적으로 사용됨.
    - 일반적인 함수(호출되어 사용되는)
    - `new` 연산자와 함께 인스턴스를 생성할 수 있는 생성자 함수
    - 객체에 바인딩된 메서드로서의 함수

```jsx
var f = function() {
	return 1;
};

f(); // 1

new f(); // f {}

var obj = { func: f };
obj.func(); // 1
```

- 위와 같이, ES6 이전의 함수는 동일한 함수지만 다양한 형태로 호출이 가능하였다.
- 또한, ES6 이전의 모든 함수는 **일반 함수로서 호출도 가능**하고, **생성자 함수로서도 호출도 가능**하였다. (**callable**이면서 **constructor**)
- 이는 (언뜻 보면) 편리해 보이지만, 실수를 유발하며 성능 면에서도 손해였다.

### 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세가지 종류로 명확이 구분했다.

1. 일반 함수(Normal)
2. 메서드(Method)
3. 화살표 함수(Arrow)

# 26.2 메서드

- **ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**

```jsx
const obj = {
	x: 1,
	f() { return this.x; }
	g: function() { return this.x; }
};

console.log(obj.f()); // 1
console.log(obj.g()); // 1
```

- 위 코드를 보면, `f`와 `g`가 하는 동작은 동일하다.
- 하지만 **ES6 메서드는 인스턴스를 생성할 수 없는, non-constructor**이다.
- 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다.

```jsx
new obj.f(); // TypeError: obj.f is not a constructor
new obj.g(); // g {}
```

- ES6 메서드는 인스턴스를 생성할 수 없으므로, `prototype` 프로퍼티도 없고, 프로토타입도 생성하지 않는다.
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.
- `super` 참조는 **내부슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로**, 내부슬롯 [[HomeObject]]를 갖는 ES6 메서드는 `super` 키워드를 사용할 수 있는 것이다.
- 반대로 말하면, ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다. 내부 슬롯 [[HomeObject]]를 갖지 않기 때문이다.

### 이처럼 ES6 메서드는 본연의 기능(super)은 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.

따라서 메서드를 정의할 때, ES6 이전의 방식은 사용하지 않는 것이 좋다. (프로퍼티 값으로 익명 함수 표현식을 할당하는 등의 방식)

# 26.3 화살표 함수

- 화살표 함수는 function 키워드 대신 화살표(`=>`, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.
- 표현만 간략한 것이 아니라, 내부 동작도 기존의 함수보다 간략하다.

### 26.3.1 화살표 함수 정의

- **함수 정의**
    - 화살표 함수는 함수 선언문으로 정의할 수 없고, 함수 표현식으로만 정의한다.
    - 호출 방식은 기존과 동일하다.
    
    ```jsx
    const multiply = (x, y) => x * y;
    multiply(2, 3); // 6 
    ```
    
- **매개변수 선언**
    - 매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언한다.
    
    ```jsx
    const arrow = (x, y) => { ... };
    ```
    
    - 매개변수가 한 개인 경우 소괄호 () 를 생략할 수 있다.
    
    ```jsx
    const arrow = x => { ... };
    ```
    
    - 매개변수가 없는 경우 소괄호 () 를 생략할 수 없다.
    
    ```jsx
    const arrow = () => { ... };
    ```
    
- ********************************함수 몸체 정의********************************
    - 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 있다.
        
        ```jsx
        const power = x => x ** 2;
        power(2); // 8
        ```
        
        이 코드는 아래 코드와 동일하다
        
        ```jsx
        const power = x => { return x ** 2};
        ```
        
    - 함수 몸체를 감싸는 중괄호 {}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다. (표현식이 아닌 문은 반환할 수 없으므로!!)
    - 이외에도 여러 유의사항이 있다.
        - 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸주어야 함
        - 함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할 수 없음. 이때 반환값이 있는 경우 명시적으로 반환해야 함
        - 화살표 함수도 즉시 실행 함수로 사용 가능

### 26.3.2 화살표 함수와 일반 함수의 차이

### 화살표 함수와 일반 함수의 차이는 다음과 같다.

1. **화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다.**
2. **중복된 매개변수 이름을 선언할 수 없다.**
3. **화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다.**

### 26.3.3 `this`

- 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 `this`이다.
- 화살표 함수는 콜백함수로 사용되는 경우가 많은데, ES6 이전까지는 “콜백 함수 내부의 this 문제”, 즉 외부 함수의 this과 콜백 함수 내부의 this가 다르기 때문에 여러 문제가 발생한다.
- 이에 ES6에서는 화살표 함수를 사용하여 해당 문제를 해결하였다.
    - 화살표 함수는 함수 자체의 `this` 바인딩을 갖지 않는다.
    - 따라서 화살표 함수 내부에서 `this`를 참조하면 상위 스코프의 `this`를 그대로 참조한다. **(lexical this)**
    - 마치 렉시컬 스코프와 같이, 함수의 this가 함수가 정의된 위치에 의해 결정됨을 의미한다.

### 26.3.4 `super`

- 화살표 함수는 함수 자체의 `super` 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 `super`를 참조하면 `this`와 마찬가지로 상위 스코프의 `super`를 참조한다.
    
    ```jsx
    class Base {
    	constructor(name) {
    		this.name = name;
    	}
    	
    	sayHi() {
    		return `Hi! ${this.name}`;
    	}
    }
    
    class Derived extends Base {
    	sayHi = () => `${super.sayHi()} how are you doing?`;
    }
    
    const derived = new Derived('Han');
    console.log(defived.sayHi()); // Hi! Han how are ~~
    ```
    
    - 이때 sayHi 클래스 필드에 할당한 화살표 함수는 함수 자체의 super 바인딩을 갖지 않으므로 super를 참조하였을 때 constructor의 super 바인딩을 참조하게 된다.
    - 위 예제의 경우 Derived 클래스의 constructor는 생략되었지만 암묵적으로 constructor가 생성된다.

### 26.3.5 `arguments`

- 화살표 함수는 함수 자체의 `arguments` 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 `arguments`를 참조하면 `this`와 마찬가지로 상위 스코프의 `arguments`를 참조한다.
- 18.2.1절에서 살펴보았듯, `arguments` 객체는 함수를 저으이할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하였다. 하지만 화살표 함수에서는 `arguments` 객체를 사용할 수 없다.
- 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때에는 반드시 Rest 파라미터를 사용해야 한다.

# 26.4 Rest 파라미터

### 26.4.1 기본 문법

- Rest 파라미터는 매개변수 이름 앞에 세 개의 점 `…` 을 붙여서 정의한 매개변수를 의미한다.
- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다

```jsx
function f(... rest) {
  console.log(rest);
} 

f(1,2,3,4,5); // [ 1, 2, 3, 4, 5 ]
```

- 일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다. 이때 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.

```jsx
function f(p, ...rest) {
	console.log(p);
	console.log(rest);
} 

f(1,2,3,4,5); // 1 ; [ 2, 3, 4, 5 ] 출력
```

- 이때 Rest 파라미터는 반드시 마지막 파라미터여야 하고, 단 하나만 선언해야 한다.

### 26.4.2 Rest 파라미터와 arguments 객체

- ES5에서는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수의 경우 함수 내부에서 지역변수처럼 사용이 가능한 arguments 객체를 활용하여 인수를 전달받았다.
- 하지만 arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열의 메서드를 사용하려면 추가 작업이 필요했다.
- 따라서 ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다. 이를 통해 유사 배열 객체인 arguments객체를 배열로 변환하는 번거로움을 피할 수 있다.

# 26.5 매개변수 기본값

- 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하지 않아도 에러가 발생하지는 않는데, 이는 JS 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다.
- ES6에서는 매개변수에 기본값을 할당하여, 인수가 전달되지 않은 매개변수에 `undefined`가 대입되는 것을 방지할 수 있다.

```jsx
function sum(x=0,y=0) {
	return x + y;
}

console.log(sum(1)); // 1, 매개변수 기본값이 없었다면 NaN이 리턴되었을 것이다
```

- Rest 파라미터에는 기본값을 지정할 수 없다.
