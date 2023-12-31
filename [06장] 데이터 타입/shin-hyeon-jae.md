# 6 데이터 타입

## 6.1 숫자타입

### 모든 숫자는 실수로 처리된다.

Java-Script는 C, Java와는 다르게 오직 실수만 제공한다.

추가로 세가지 특별한 값도 표현된다. (대문자, 소문자 구별 철저)

- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : 산술 불가

```JavaScript
console.log(10/0);              // infinity
console.log(10/-0);             // -infinity
consloe.log(1 * 'asdfasdf');    // NaN
```

## 6.2 문자열 타입

### 모든 문자는 작은 따옴표(' '), 큰 따옴표(" "), 벡틱(\` \`)으로 감싼다.

가장 일반적인 Java-Script문법은 작은 따옴표(' ')를 사용하는것이다.

```JavaScript
var string;
string = '문자열';
string = "문자열";
string = `문자열`;
```

## 6.3 템플릿 리터럴

### 템플릿 리터럴은 멀티라인 물자열, 표현식 삽입, 태그드 탬플릿등 편리한 문자열 처리 기능을 제공한다. 템퍼럴 문자열은 백틱(\` \`)을 사용한다.

### 6.3.1 멀티라인 문자열

템퍼럴 문자열에서는 '\n'와 같은 줄바꿈 문자열이나 '\t'같은 공백문자열을 사용할 필요없으며 줄바꿈이나 모든 공백이 허용된다.

```JavaScript
var temp = 'Hello.\n My name is shin hyeon jae.\n\t Nice to meet you!';
console.log(temp);
```

```
out:
Hello.
 My name is shin hyeon jae.
     Nice to meet you!
```

```JavaScript
var temp = `Hello.
 My name is shin hyeon jae.
     Nice to meet you!`;
console.log(temp);
```

```
out:
Hello.
 My name is shin hyeon jae.
     Nice to meet you!
```

따라해본다면 위의 출력과 같이 똑같이 출력되는 것을 볼 수 있을 것이다.

### 6.3.2 표현식 삽입

문자열은 문자열 연산자 +를 사용해 연결할 수 있다. 피연산자중 하나 이상이 문자열인 경우 +연산자는 연결 연산자로 동작한다.

```JavaScript
var first = 'asdf';
var second = 'bbbbbbbbb';

console.log('wwww'+ ' ' + first + ' ' + second + '.');
```

```
out:
wwww asdf bbbbbbbbb.
```

표현식을 삽입하려면 ${ }으로 표현식을 감싼다.

```JavaScript
var first = 'asdf';
var second = 'bbbbbbbbb';

console.log('wwww ${first} ${second}.');
```

```
out:
wwww asdf bbbbbbbbb.
```

## 6.4 불리언 타입

### 불리언 타입은 참, 거짓만의 값을 가지는 자료형이다.

## 6.5 undefined 타입

### undefined는 개발자가 정의하지 않았다는 것을 알려주는 자료형이다.

## 6.6 null 타입

### 프로그래밍언어에서 변수에 아무 값이 없다는 것을 의도적으로 명시하기 위해 만든 자료형이다.

### undefined와 혼동하기 쉬우니 주의하자

## 6.7 심벌 타입

### 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기위해 사용한다.

```JavaScript
//심벌 값 생성
var key = Symbol('key');
console.log(typeof key);

// 객체 생성
var obj = {};

obj[key] = 'value'
console.log(obj[key]);
```

```
out:
symbol
value
```
