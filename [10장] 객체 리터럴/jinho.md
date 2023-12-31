## 10.1 객체란?

* 자바스크립트에서 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식등)은 모두 객체이다. <br>
`정규표현식은 문자열에서 특정 패턴을 찾거나 매칭시키기 위한 표현식`

* 객체 타입(object type)은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이다.

* 원시 타입의 값과는 다르게 객체 타입의 값은 변경 가능하다.

* 객체는 0개 이상의 프로퍼티로 구성되어 있다. 

프로퍼티 구조
```
var person = {
  name : ' park',
  age: 22
  // name과 age는 프로퍼티 키 이고  'park'와 22는 프로퍼티 값이다.
}
```
프로퍼티 값이 함수일 때 일반 함수와 구분하기 위해 메서드라 부른다.

프로퍼티: 객체의 상태를 나타내는 값(data)<br>
메서드: 프로퍼티를 참조하고 조작할 수 있는 동작<br>
=> 객체는 이 둘을 모두 포함할 수 있기에 상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.

## 10.2 객체 리터럴에 의한 객체 생성

자바스크립트는 c++나 자바 같은 클래스 기반 객체지향 언어가 아니라 프로토타입 기반 객체지향 언어이다.<br>
따라서 클래스를 사전에 정의하지 앟아도 되고 필요한 시점에 new연산자와 함께 생성자를 호출하지 않아도된다.<br>
또한 클래스 기반 언어와 달리 다양한 방법으로 객체를 생성할 수 있다. 

그중  가장 간단한 방법인 객체 리터럴을 사용하는 방법에 대해 알아보겠다.<br>
* 객체 리터널 외의 객체 생성 방식은 모두 함수를 사용해 객체를 생성한다.
* 중괄호내에 **0개** 이상의 프로퍼티를 정의한다.
* 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다. 
* 중괄호는 코드 블록을 의미하지 않기에 닫는 중괄호 뒤에 세미콜론을 붙인다.
* 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수도 있다.
* 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.  => 유연하고 강력한 방식인 이유

## 10.3 프로퍼티

프로퍼티 생성 규칙<br>
* 프로퍼티를 나열할 때는 쉼표로 구분한다. 
* 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 한다. <br>
따라서 심벌 값, 문자열을 프로퍼티 키로 사용  일반적으로 문자열을 프로퍼티 키로 사용한다. <br>
=> 일반적으로 프로퍼티 키는 문자열이기에 따옴표로 묶어야하나 식별자 네이밍 규칙을 지킨 경우에는 따옴표를 사용하지 않아도된다.
```
var person = {
  firstName: "jinho",  // 식별자 네이밍 규칙을 준수한 경우
  'last-name':'park'  // 식별자 네이밍 규칙을 준수하지 않은 경우

}
```
*  대괄호를 통해 문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다. 이 경우에 표현식을 대괄호로 묶어야한다.
```
var obj = {};  // 빈 객체 생성 
var key = 'hello';  // 프로퍼티 키 값으로 사용할 표현식 
// 프로퍼티 키 동적 생성
obj[key] = 'world';
console.log(obj);  // {hello: "world"}
```
* 빈 문자열을 프로퍼티 키로 사용해도 에러가 발생하지는 않으나 키로서의 의미를 가지 못한다.
* 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다. <br>
`암묵적 타입 변환: 코드 문맥에 타입이 맞지 않아 자바스크립트 엔진이 강제로 타입을 변환시킴`
* var, function과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않지만 예상치 못한 에러가 발생할 여지가 있으므로 권장하지 않는다.

* 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다. 이때 에러가 발생하지 않는다는 점 주의
```
var foo = {
  name: 'jinho',
  name: 'jaino'
}
console.log(foo);  // {name: "jaino"} 출력
```

## 10.4 메서드

메서드는 객체에 묶여 있는 함수를 의미한다.<br>
자세한 내용은 12장에서 설명하기에 어떻게 사용되는지만 간단히 살펴보겠다.
```
var circle = {
  radius: 5, // 프로퍼티

  getDiameter: function(){   // 메서드
    return 2 * this.radius;  // this는 circle을 가리킨다.
  }
};
```

## 10.5프로퍼티 접근

프로퍼티에 접근하는 방법에는 두 가지가 있다.<br>
1. 마침표 프로퍼티 접근 연산자를 사용하는 마침표 표기법<br>
프로퍼티 키가 식별자 네이밍 규칙을 준수한 이름인 경우에만 사용가능<br>
 형식: 객체이름.프로퍼티 키 

2. 대괄호 프로퍼티 접근 연산자를 사용하는 대괄호 표기법<br>
대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야한다. <br>
따옴표로 감싸지 않은 경우 식별자로 해석된다. <br>
식별자 네이밍 규칙을 지키지 않은 경우 대괄호 프로퍼티 접근 연산자를 통해 접근 가능<br>
형식: 객체이름['프로퍼티 키']

## 10.6 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하며 프로퍼티 값이 갱신되다.
```
var person = {
  name: 'lee'
};
person.name = 'kim'
``` 


## 10.7 프로퍼티 동적 생성
존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.
```
var person = {
  name: 'Lee'
};
person.age =20;
// name: 'Lee', age: 20 출력된다.
```

## 10.8 프로퍼티 삭제
delete연산자를 통해 객체의 프로퍼티를 삭제한다. delete연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다. 

```
var person = {
  name: 'Lee'
};
delete person.name;
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약 표현
프로퍼티 값은 변수에 할당된 값, 즉 식별자 표현식을 수도 있다.<br> 
따라서 프로퍼티 키와 프로퍼티 값에 사용되는 변수가 동일한 이름일 때 프로퍼티 키를 생략가능

```
let x = 1 , y =2;
//프로퍼티 축약 표현
const obj = {x, y};
// {x: 1, y: 2}   // 프로퍼티 키는 변수 이름으로 자동 생성
```

### 10.9.2 계산된 프로퍼티 이름
문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.<br>
단 프로퍼티 키로 사용할 표현식을 대괄로 묶어야 한다. 이를 계산된 프로퍼티 이름이라고 한다. 
<br>`p135 10-22 예제 참고`

### 10.9.3 메서드 축약 표현
메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다. <br>
축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수화 다르게 동작한다. 이는 26.2절에서 자세히 다룬다.

```
var circle = {
  radius: 5, // 프로퍼티

  getDiameter() {   // function 생략가능
    return 2 * this.radius;  // this는 circle을 가리킨다.
  }
};
```
