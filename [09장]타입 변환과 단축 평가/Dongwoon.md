# 9.4 단축 평가
## 9.4.1 논리 연산자를 사용한 단축 평가
논리 연산자란 ||(논리합)와 &&(논리곱)가 있다. 

&&(논리곱)는 두 개의 피연산자가 모두 참일 경우 참을 반환한다.

||(논리합)는 두 개의 피연산자 중 하나만 참이어도 참을 반환한다.

만약 'rainy' && 'sunny'의 경우, rainy는 truthy값(true로 보는 값)이고 sunny가 truthy일때, true를 반환한다.

그러나, 논리곱 연산자는 두 번째 피연산자(sunny)를 그대로 반환한다.

논리합 연산자는 논리곱과 다르게 첫 번째 피연산자 값이 truthy값일 경우, 첫 번째 피연산자 값을 그대로 반환한다.

이렇게 결과값을 결정하는 피연산자를 반환하는 과정을 **단축 평가**라고 한다.

**단축 평가**는 미리 논리연산 과정이 확정된 경우 나머지 과정을 단축시켜 나타내는 원리이다.

즉, 'rainy' || 'sunny'의 경우에 rainy가 truthy이고 논리합 연산자를 사용했기 때문에 두 번째 피연산자가 무엇이오든 true 값이 존재하므로 rainy를 반환하는 것이다.

- true && anything = anything

- false && anything = false

- true || anything = true

- false || anything = anything

이 **단축 평가**는 if문으로 대체가 가능하다.
```javascript

var rain = true;
var message = ' ';

message = rain && '비';
console.log(message); // => '비'
```

위 코드의 경우 if문을 사용하지 않고 rain변수가 true일 경우, message 변수(anything)가 '비'라는 값을 도출해내도록 논리곱 연산자로 대체해서 사용이 가능하다. 

단축평가는 if문 외에도 객체가 가리키는 변수가 null인지 아닌지를 확인하고 프로퍼티를 참고할 때도 사용된다.

```javascript

var sun = null;
var value = sun.vaule; // => TypeError
//value 변수를 참조하기 위한 sun의 value가 null일 경우 TypeError가 발생
```

```javascript

var sun = null;
var value = sun && sun.vaule; // => null
// &&를 사용해 sun이 falsy 값이므로 단축되어 자동적으로 false값(null) 출력
```

이외에도 함수 매개변수의 기본값을 설정할때도 단축 평가가 사용된다.

```javascript

function getlength(str);{
str = str || ' ';
return str.length;
}
getlength('hello'); // => 5
// hello || ''는 true || anything 이므로 단축평가되어 true(hello)가 도출되므로 str.length를 통해 str의 길이(=hello의 길이) 5를 출력된다.
```

## 9.4.2 옵셔널 체이닝 연산자

옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null or undefined일 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어가는 연산자이다.

```javascript
var elem = null;
var value = elem?.value;
console.log(value);
// elem이 null값을 가지므로 undefined를 반환
```
옵셔널 체이닝 연산자는 변수 값이 null or defined인지 아닌지 확인하기 위함이며 도입되기 이전에는 논리연산자 &&를 이용한 단축 평가를 사용해 평가하였다.
```javascript

var sun = '';
var length = str?.length;
console.log(length);
// sun의 값이 null or defined 값이 아니기 때문에 우항의 값인 0을 반환
```

## 9.4.3 null 병합 연산자

null 병합은 ??로 표현하며 좌항의 피연산자가 null or undefined인 경우 우항의 피연산자를 반환하고 그렇지 않으면 좌항의 피연산자를 반환한다.

```javascript

var sun = null ?? 'default string';
console.log(sun); // => default string
// 좌항의 값이 null값을 가지므로 우항의 값을 출력한다. 
```
null병합은 기본값을 설정할 때 유용하며 도입되기 이전에는 || 연산자를 이용한 단축평가로 설정하였다.

```javascript

var sun = '' ?? 'default string';
console.log(sun); // =>''
// 좌항의 값이 null과 같은 falsy값이어도 null이 아니므로 좌항의 ''를 반환하였다.
```
# 10.1 객체란?

자바를 구성하는 모든 것을 객체라고 부를 수 있으며 변경 가능한 값인 특징이 있다.

객체는 여러 개의 프로퍼티로 구성되어 있으며 프로퍼티는 키(key)와 값(value)으로 구성된다.

```javascript

var book = {

name = '이산수학',
total pages = 313
};
//{}안에 있는 코드들을 프로퍼티라고 하며 이때 total pages는 프로퍼티 키, 313은 프로퍼티 값이라고 할 수 있다.
```
자바는 모든 것이 객체가 될 수 있으므로 함수도 프로퍼티로 표현할 수 있는데 이를 메서드라고 부른다.

프로퍼티 : 객체의 상태를 나타내는 값

메서드 : 프로퍼티를 조작할 수 있는 동작

# 10.2 객체 리터럴에 의한 객체 생성

자바스크립트에서의 객체를 생성하기 위해서는 다양한 방법이 존재하는데 가장 보편적인 방법은 객체 리터럴이다.

객체 리터럴은 객체를 만들기 위한 약속된 기호라는 의미로 {} 내에 프로퍼티들을 정의하면 자바스크립트 엔진은 이를 해석해 객체를 생성하는 원리이다.

만약 {}안에 아무 프로퍼티를 정의하지 않는다면 object타입이라는 빈 객체를 생성한다.

객체 리터럴 방식은 자바스크립트에서 가장 유용하게 쓰이는 방식이며 객체를 생성함과 동시에 프로퍼티를 선언할 수 있다는 차별점이 있다.

# 10.3 프로퍼티

프로퍼티는 키(key)와 값(value)로 구성되어 있으며 객체 내의 프로퍼티들을 나열할 때는 ,를 사용한다.

프로퍼티 키는 프로퍼티 값에 접근할 수 있는 말 그대로 열쇠같은 존재이다. 

프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열이자 심벌 값, 함수(식별자)같은 역할

프로퍼티 값 : JS에서 사용 가능한 모든 값

이때, 프로퍼티 키는 식별자같은 역할을 하지만 식별자 네이밍 규칙을 반드시 따르지 않다.

- 프로퍼티 키가 식별자 네이밍 규칙을 따른다면 따옴표를 생략 가능하다. 

   즉, 식별자 네이밍 규칙을 준수하지 않을 경우 따옴표는 생략 불가능하다.

```javascript

var per = {
first : 'dong woon',
'last-name' : 'woon'
//'-'가 붙어 식별자 네이밍 규칙을 준수하지 않으므로 따옴표를 붙여 프로퍼티 키를 만들었다.
// 준수하지 않고 따옴표를 붙이지 않는다면 프로퍼티 키로 보는 것이 아닌 -(마이너스)연산자로 구성된 식으로 본다.
};

```
- 이외에도 문자열을 포함한 식을 프로퍼티 키로 사용할 수 있다. 이 경우에는 표현식(식별자)를 대괄호로 묶는다.
```javascript

 var out = {};
 var in = 'tired';
out[[in] = 'sleep';
// output = tired : 'sleep'
```
- 빈 문자열도 프로퍼티 키로 사용할 수 있으나 판별할 수 없어 사용하지는 않는다.

- 프로퍼티 키에 문자열외의 값을 사용하면 암묵적으로 타입 변환이 일어나 문자열이 된다.
```javascript
 var out = {
 1 : 3
};
// output = 1 : 3
```
- function과 같은 이미 설정되어 있는 예약어를 프로퍼티 키로 사용해도 괜찮지만 가독성의 문제로 쓰지 않는다.

- 프로퍼티 키를 중복으로 사용할 경우 나중에 선언된 프로퍼티가 모두 덮어써 출력된다.

