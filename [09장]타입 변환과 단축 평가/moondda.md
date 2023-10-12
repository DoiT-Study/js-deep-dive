## 9.1 타입 변환이란?

**명시적 타입 변환**
타입 캐스팅(type casting)이라고도 한다
개발자가 의도적으로 값의 타입을 변환하는 것

**암묵적 타입 변환**
타입 강제 변환이라고도 한다.
개발자의 `의도와 상관없이` 표현식을 평가하는 도중, 자바스크립트 엔진에 의해 타입이 변환되는 것.

타입 변환이라고 해서 기존의 원시 값을 직접 변경하는 것 X!
원시 값은 변경 불가능한 값(immutable value)이기 때문.
그렇다면 무엇인가?

-> `기존 원시 값을 사용해 <다른 타입>의 새로운 원시 값을 생성하는 것`

``` javascript
var age = 23;
age.toString();

console.log(typeof age, age); //number 23
```

``` javascript
var age = 23;
var age_str = age.toString();

console.log(typeof age, age); //number 23
console.log(typeof age_str,age); //string 23
```

## 9.2 암묵적 타입 변환

암묵적 타입 변환이 발생가면 원시 타입 중 하나(문자열,숫자,불리언,..)로 타입이 자동 변환된다

### 9.2.1 문자열 타입으로 변환

``` javascript
1 + '2' // "12"
true + '' //"true"
undefined + '' //"undefined"
```
+ 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.
  

``` javascript
1 + 2 + 3 +'4';
1 + 2 + '3' + '4';
1+ '2' + 3 + '4';
```


### 9.2.2 숫자 타입으로 변환

-,*,/ 와 같은 산술 연산자의 역할은 **숫자 값 만들기** 이기 때문에 코드 문맥상 Number type이 적절하다.
만약 Number type으로 변경할 수 없을 시 평과 결과는 NaN이 된다

=,>,< 와 같은 비교연산자 Boolean 값을 만드는 것이기 때문에, 크기 비교를 위해 피연산자가 Number type이어야 한다.

``` javascript
1 * "5000" //5000
"1" > 0 //true
```

 +단항 연산자는 숫자 타입이 아닌 값을 숫자 타입으로 암무적 타입 변환을 수행해준다

``` javascript
+"1" //1
+"string" //NaN
+true //1
+null //0
```

빈 문자열(''), 빈 배열([]), null, false는 0으로 변환
객체, 빈 배열이 아닌 배열, undefined는 변환이 되지 않음 -> NaN


### 9.2.3 불리언 타입으로 변환
조건식의 평과결과는 자바스크립트 엔진은 boolean 타입으로 암묵적 타입 변환한다
이때 엔진은 불리언 타입이 아닌 값을 Truthy값, Falsy값으로 구분한다

Truthy값: 참으로 평가되는 값
- Falsy가 아닌 값

Falsy값: 거짓으로 평가되는 값 
- false
- undefined
- null
- 0,-0
- NaN
- ''(빈문자열)


## 9.3 명시적 타입 변환

- 표준 빌트입 생성자 함수(String, Number, Boolean)을 new 연산자 없이 호출
- 빌트인 메서드 사용
- 암묵적 타입 변환 이용

### 9.3.1 문자열 타입으로 변환

``` javascript
String(1);
(1).toString();
1+'';
```

### 9.3.2 숫자 타입으로 변환

``` javascript
Number("1");//1
parseInt("1");//1
parseFloat("10.57");//10.57
+'8'; //8
'8' * 1 ; //8
```

### 9.3.3 불리언 타입으로 변환

``` javascript
Boolean('x') //true
Boolean('false') //true
!!0 //false
!!1// true
!!null //false
```
내가 이해한 방식
-> !!가 부정의 부정이니까 긍정. 결국 그 값은 유지가 되는데 형변환만 시켜주는 것. !!을 이용하여 명시적으로

## 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가
논리합(||) 논리곱(&&) 연산자 표현식은 `2개의 피연산자 중 어느 한쪽으로` 평가된다

``` javascript
"Apple" || "Orange" //true
"Apple" && "" //""
false && "Apple" //false
```
단축평가 규칙 p119 참고

### 9.4.2 옵셔널 체이닝 연산자
### 9.4.3 null 병합 연산자
  
