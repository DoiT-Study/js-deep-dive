## 19.8 오버라이딩과 프로퍼티 섀도잉

오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

프로퍼티 섀도잉: 상속 관계에 의해 프로퍼티가 가려지는 현상

프로토타입의 프로퍼티를 변경 또는 삭제<br>
: 하위 객체를 통해 프로토타입에 접근하는 것이 아닌 **직접** 접근해야 한다.

## 19.9 프로토타입의 교체
* 프로토타입은 임의의 다른 객체로 변경할 수 있다<br>
=> 프로토타입 동적으로 변경<br>
=> 객체 간의 상속 관계를 동적으로 변경 가능
* 생성자 함수 또는 인스턴스에 의해 교체가능

### 19.9.1 생성자 함수에 의한 프로토타입의 교체
```
const Person = (funciton () {
  founction Person(name){
    this.name = name;
  }


  Person.prototype = {
    SayHello(){
      console.log('Hi');
    }
  };

  return Person;
}());

const me = new Person('Lee');

```
=> Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체
* 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다.<br> 
=> constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.<br> 
=> 위의 예시에서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.
* 연결 되살리기 : 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가
* 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것 

`constructor 프로퍼티: 자바스크립트 엔진이 프로토타입을 암묵적으로 추가한 프로퍼티`

### 19.9.2 인스턴스에 의한 프로토타입의 교체
* 인스턴스의__ proto __ 접근자 프로퍼티를 통해 접근
* __ proto __ 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것
* 생성자 함수의 프로토타입이 교체된 프로토타입을 가리키지 않는다.
* 연결한 객체 consturctor 프로퍼티가 없으면 생성자 함수와 constructor 프로퍼티 간의 연결이 파괴된다.


=> 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭다.  => 직접 교체하지 않는 것이 좋다. 

## 19.10 instanceof 연산자
instanceof 연산자
* 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다.
* 우변의 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다. 
* 생성자 함수의 prototype에 바인딩된 객체가 **프로토타입 체인** 사이에 존재하는지 확인한다. => constructor 프로퍼티가 없어도 상관없음


## 19.11 직접 상속
### 19.11.1 Object.create에 의한 직접 상속
Object.create 메서드: 명시적으로 프로토타입을 지정하여 새로운 객체를 생성
* 메서드의 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달
* 두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달 
* 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성
=> 직접적으로 상속을 구현

장점
* new 연산자가 없이도 객체를 생성할 수 있다.
* 프로토차입을 지정하면서 객체를 생성할 수 있다.
* 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

### 19.11.2 객체 리터럴 내부에서 __proto__에 의한 직접 상속
위의 Object.create 메서드를 사용할 때 두 번째 인자로 프로퍼티를 정의하는 것이 번거롭다<br>
=> 객체 리터럴 내부에서__proto__접근자 프로퍼티를 사용하여 직접 상속 구현

## 19.12 정적 프로퍼티/메서드
* 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드
*  staticPop, staticMethod를 통해 생성
* 생성자 함수가 새성한 인스턴스로 참조/호출할 수 없다.<br>
why: 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근할 수 없다.

## 19.13 프로퍼티 존재 확인
### 19.13.1 in 연산자
* 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인
* key in object 형식으로 사용
* 확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인함
### 19.13.2 Object.prototype.hasOwnProperty메서드
전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환

## 19.14 프로퍼티 열거
### 19.14.1 for...in 문
* for ( 변수선언문 in 객체){...}
* 객체의 프로퍼티 개수만큼 순회하며 for...in문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당
* in 연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다.
* 프로퍼티 어트리뷰트[[Enumerable]]가 열거 가능 여부를 나타낸다.<br>
=> 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거
* 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
* 프로퍼티를 열거할 때 순서를 보장하지 않는다.

### 19.14.2 Object.keys/values/entires 메서드
for in 문은 객체 자신의 고유 프로퍼티뿐만 아니라 상속받은 프로퍼티도 열거한다.<br>
=> Object.prototype.hasOwnProperty메서드를 통해 객체 자신의 프로퍼티인지 확인하는 추가 처리 필요

객체 자신의 고유 프로퍼티만 열거하기 위해서는 Object.keys/values/entires 메서드 사용 권장
