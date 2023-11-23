# 25장 클래스
## 25.1 클래스는 프로토타입의 문법적 설탕인가 ?
자바스크립트는 프로토타입 기반 객체지향 언어이므로 클래스가 필요 없다.

그래서 클래스 기반 객체지향 프로그래밍 언어와 흡사하지만 새로운 객체 생성 메커니즘을 사용한다.

즉, 클래스는 자바스크립트에서 함수와 같아 문법 설탕으로도 볼 수 있다.

문법 설탕은 읽는 사람, 적은 사람이 쉽게 볼 수 있도록 디자인된 문법이다.

## 25.2 클래스 정의
클래스는 class라는 키워드를 사용해 선언한다. 

아래의 코드에서 익명 / 기명으로도 표현이 가능한데 이는 함수와 마찬가지로 이름의 유무 또한 구별시킬 수 있다.
```javascript
class name{} // 클래스 선언
const name = class {}; // 익명 클래스
const name = class hand{}; // 기명 클래스
```
클래스 몸체에는 0개 이상의 메서드만을 정의할 수 있으며 그 중에서도 생성자, 프로토타입 메서드, 정적 메서드를 사용 가능하다.
```javascript
class person{ // 클래스 선언
constructor(name){ // 생성자 
 this.name = shin; }

hi(){
console.log('hello my name is ${this.name}'); } // 프로토타입 메서드

static hello(){
cosole.log('hello');} // 정적 메서드
}
```
## 25.3 클래스 호이스팅
클래스는 일종의 함수이다.

따라서, 클래스는 함수처럼 런타임 전에 미리 평가되는 과정을 거친다.

이때, 클래스는 정의 이전에는 참조할 수 없다.
```javascript
console.log(name);
class name{}
```
또한, 클래스는 함수와 마찬가지로 호이스팅이 일어난다는 특징이 있다. 
```javascript
const person = '';
{
  console.log(person); // ''가 출력되지 않고 referenceError 발생
  class person{}
}
```
## 25.4 인스턴스 생성
클래스는 생성자 함수로서 new연산자와 함께 인스턴스를 생성한다.
```javascript
class person{
const you = new person();
console.log(you); // person{}
```
클래스의 존재 이유는 인스턴스 생성이기 때문에 필연적으로 new연산자와 함께 쓰인다.

함수 표현식처럼 클래스를 가리키는 식별자를 사용해 인스턴스를 사용해야 하는데 그렇지 않은 경우에는 에러가 발생한다.
```javascript
const person = class myclass{};
const me = new person();
const you = new myclass(); // referenceError(myclass is not defined)
// 클래스 표현식에서 사용한 클래스 myclass는 외부 코드에서 접근 불가능하다.
```
## 25.5 메서드
### 25.5.1 constructor 메서드
클래스에서의 메서드 중 constructor(생성자)는 인스턴스를 생성 / 초기화하기 위한 메서드이다.

즉, 생성자는 new연산자를 통해 자동으로 호출되어 인스턴스를 초기화가 가능하다.
```javascript
const me = new person('lee'); // constructor : class person
console.log(me);
```
constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.
```javascript
const person{
 constructor(name){
 this.name= name;
 }
}
function person(name) {
this.name = name;
}
```
이때, 위의 코드에서 클래스가 생성한 인스턴스에서는 constructor 메서드를 찾아볼 수 없는데 이는 constructor가 메서드로 해석되는 것이 아니다.

constructor는 클래스가 생성한 함수 객체 코드의 일부가 되는 것이다.

또한, constructor는 생성자 함수와 유사하지만 몇 개의 차이가 존재한다.

그 중 constructor는 클래스 내에서 한 개만 존재할 수 있다는 것이다.

constructor는 생략이 가능한데 이 경우 암묵적으로 정의되어 빈 constructor에 의해 빈 객체가 생성된다.
```javascript
class person {
 constructor () {}
}
const me = new person(); // 빈 객체 생성
console.log(me); // person {}
```
프로퍼티를 추가하기 위해 인스턴스를 생성하려면 constructor 내부에의 this에 인스턴스 프로퍼티를 추가하면 된다.

인스턴스를 초기화 하기 위해서는 constructor를 생략해서는 안된다는 것이다.
### 25.5.2 프로토타입 메서드
클래스 몸체에서의 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 프로토타입 메서드가 된다.
```javascript
class person {
sayhi() { // 프로토타입 메서드
  console.log('hi, my name is ${this.name}');
 }
}
const me = new person('lee');
me.sayhi(); // hi, my name is lee
```
따라서 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다. 

클래스는 생성자 함수의 역할인 인스턴스 생성을 하는 생성자 함수라고도 생각해볼 수 있다.
### 25.5.3 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출 가능하다.

클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드가 된다.
```javascript
class person {
constructor(name) {
this.name - name;
}
static hello() {
console.log('hello');
 }
}
person.hello(); // hello
```
위의 코드에서 마지막 줄을 보면 인스턴스가 아닌 클래스를 호출하였다. 이처럼 프로토타입 메서드와는 다르게 정적 메서드는 클래스로 호출한다.
### 25.5.4 정적 메서드와 프로토타입 메서드의 차이
위에서 말했듯이 정적 메서드는 클래스로 호출되고 프로토타입 메서드는 인스턴스로 호출된다.

또한, 정적 메서드는 인스턴스 프로퍼티 참조가 불가능하지만 프로토타입 메서드는 인스턴스 프로퍼티 참조가 가능하다.

## 25.6 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환
```javascript
class person {
constructor(name) { // 암묵적으로 인스턴스 생성, this바인딩
console.log(this); // person {}
console.log(Object.getPrototypeOf(this) === person.prototype); // true

this.name = name; // 2. this 바인딩 되어있는 인스턴스를 초기화

//3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
 }
}
```


