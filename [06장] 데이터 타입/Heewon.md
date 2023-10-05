# 6. 데이터 타입

자바스크립트 ES6 버전은 7개의 데이터 타입을 가지며, 이는 또다시 원시 타입과 객체 타입 두 가지로 분류된다. 타입별로 확보해야 할 메모리 공간, 저장되는 2진수 값, 읽어들이는 방식 등이 모두 다르기에 우리는 명확한 의도를 가지고 타입을 구별하여 변수, 그리고 그에 해당하는 값을 생성해야 한다.

## 6.1. 숫자 타입 Number Type

정수와 실수의 구분 없이 모두 하나의 숫자 타입으로 할당하여 실수로 처리한다. 다음은 모두 숫자 타입이며, 정수로 표시되어 있더라도 실수 타입으로 연산한다.

```
var integer = 10;
var double = 10.12;
var negative = -20;

console.log(4 / 2); // 2
console.log(4 / 3); // 1.5
```

<br>

2진수, 8진수, 16진수 또한 같은 타입이기에 해석할 때는 모두 10진수로 해석된다.

```
var binary = 0b01000001;
var octal = 0o101;
var hex = 0x41;

console.log(binary);   // 65
console.log(octal);    // 65
console.log(hex);      // 65
```

<br>

양의 무한대, 음의 무한대, 산술 연산 불가 세 가지 특별한 값도 표시 가능하다. NaN의 경우 대소문자에 주의해야 한다.

```
console.log(10 / 0);        // Infinity
console.log(10 / -0);       // -Infinity
console.log(1 * 'String');  // NaN
```

## 6.2. 문자열 타입 String Type

- 작은따옴표 ( ' ' )
- 큰따옴표 ( " " )
- 백틱 (백코트) ( `` )

문자열을 따옴표로 감싸지 않으면 키워드나 식별자와 같은 토큰으로 인식되며, 공백 문자도 포함시킬 수 없다. 또한 문자열은 생성된 후 그 값을 변경할 수 없다.

```
var string;

string = '작은따옴표로 감싼 문자열 내의 "큰따옴표"는 문자열로 인식된다.';
stirng = "큰따옴표로 감싼 문자열 내의 '작은따옴표'는 문자열로 인식된다.";

string = hello; // ReferenceError: hello is not defined
```

## 6.3. 템플릿 리터럴 Template Literal

ES6부터 편리한 문자열 처리를 위해 새로운 문자열 표기법이 도입되었다. 일반적인 따옴표 대신 백틱(또는 백코트)을 사용해 표기한다.

### 6.3.1. 멀티라인 문자열 Multi-line String

일반 문자열 내에서는 줄바꿈을 위해 개행문자(`\n`)을 사용해야 하지만 템플릿 리터럴 내에서는 개행문자 없이도 줄바꿈과 공백이 있는 그대로 적용된다.

<br>

다음은 일반 문자열의 예시와 그 출력이다.

```
var template = '<ul>\n\t<li><a href="#">Home</a></li>\n</ul>';
console.log(template);
```

```
<ul>
    <li><a href="#">Home</a></li>
</ul>
```

<br>

다음은 템플릿 리터럴 내에서의 예시와 그 출력이다.

```
var template = `<ul>
    <li><a href="#">Home</a></li>
</ul>`;
console.log(template);
```

```
<ul>
    <li><a href="#">Home</a></li>
</ul>
```

### 6.3.2. 표현식 삽입 Expression Interpolation

ES5까지는 문자열 연산자 '+'를 사용해 연결해야 했으나, ES6부터는 템플릿 리터럴 내 표현식 삽입을 통해 보다 가독성 높고 간편한 방식으로 문자열을 연결한다.

```
var first = 'Heewon';
var last = 'Jeon';

// ES5
console.log('My name is ' + first + ' ' + last + '.'); // My name is Heewon Jeon.

// ES6
console.log(`My name is ${first} ${last}.`); // My name is Heewon Jeon.
```

<br>

표현식에 연산식을 넣을 수도 있다. 이때 결과가 문자열이 아니더라도 문자열로 타입이 강제 변환되어 삽입된다.

```
console.log(`1 + 2 = ${1 + 2}`);    // 1 + 2 = 3 
```

## 6.4. 불리언 타입 Boolean Type

논리적 참, 거짓을 나타내는 `true`와 `false` 두 개가 있다.

## 6.5. undefined 타입

var 키워드로 선언한 변수는 암묵적으로 undefined로 초기화된다. 이 타입을 개발자가 의도적으로 변수에 할당한다면 혼란을 야기할 수 있다.

## 6.6. null 타입

변수에 값이 없다는 것을 표시하고 싶다면 undefined가 아닌 null을 할당한다. 대소문자에 주의해야 한다. 또한 함수가 유효한 값을 반환하지 못하는 경우 에러 대신 null을 반환한다.

```
<!DOCTYPE html>
<html>
<body>
    <script>
        var element = document.querySelector('.myClass');

        // HTML 문서에 myClass 클래스를 갖는 요소가 없다면 null을 반환한다.
        console.log(element);   // null
    </script>
</body>
</html>
```
<br>

# 느낀 점 & 토론

'변수에는 타입이 없고 값에 타입이 있으며, 값이 할당됨에 따라 그에 맞는 타입을 변수에 지정해주는 것이다' 라는 문장을 읽고 파이썬과 자바스크립트 변수의 본질을 알게 된 느낌이 들었다. 명확한 개념을 모르고 적당히 섞어 썼던 용어들의 의미를 알게 된 것이 오늘의 의의라고 생각한다. 여느 프로그래밍 언어가 그렇듯, 조금이라도 손쉬운 개발이 가능하도록 패치한 노력이 이곳저곳에서 드러나는 언어였다. 또한 템플릿 리터럴이 무엇인지 정확하게 모르고 쓰느라 백코트와 따옴표를 헷갈려 애먹었던 경험이 있는데, 그 궁금증을 해결할 수 있었다.

<br>

자바스크립트의 원시 타입이 타 언어의 원시 타입보다 부족한 점은 무엇일까? 예를 들어 C에 익숙한 사람은 정수와 실수의 구분이 없는 것에 대해 불편함을 느낄지도 모른다. (정수 간의 나눗셈에서 자동 반올림이 되지 않는 등)
