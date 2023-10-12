# 9. 타입 변환과 단축 평가

## 9.1. 타입 변환이란?

타입 변환이란 기존 원시 값을 변경하는 것이 아니다. 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다. 이렇게 생성된 값은 단 한 번 사용되고 버려진다.

- 명시적 타입 변환 (Type casting) : 개발자가 의도적으로 값의 타입을 변환하는 것

``` jsx
var x = 10;

var str = x.tostring();
console.log(typeof str, str); // string 10

console.log(typeof x, x); // number 10, x 변수의 값이 변경된 것은 아니다
```

- 암묵적 타입 변환 (Type coercion) : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것

``` jsx
var x = 10;

var str = x + '';
console.log(typeof str, str); // string 10

console.log(typeof x, x); // number 10, x 변수의 값이 변경된 것은 아니다
```

명시적 타입 변환은 타입을 변경하겠다는 개발자의 의지가 코드에 명백히 드러나는 반면, 암묵적 타입 변환은 자바스크립트 엔진에 의해 타입이 자동 변화되기에 타입을 변경하겠다는 개발자의 의지가 드러나지 않는다.

따라서 자신이 작성한 코드에 암묵적 타입 변환이 발생하는지, 그에 따른 결과는 무엇인지 예측 가능해야 한다. 이는 '코드는 개발자가 알아듣기 쉬워야 한다'는 원칙과 일맥상통한다.

## 9.2. 암묵적 타입 변환

자바스크립트에서는 코드의 문맥에 따라 데이터 타입을 강제 변환하는 경우가 있다.

### 9.2.1. 문자열 타입으로 변환

``` jsx
1 + '2' // "12"

`1 + 1 = ${1 + 1}` // "1 + 1 = 2"
```

첫 번째 예시의 피연산자 중 하나 이상이 문자열이므로 `+` 연산자는 문자열 연결 연산자로 동작한다. 숫자 타입이었던 1은 문자열 '1'로 강제 변환된다.

두 번째 예시인 템플릿 리터럴의 표현식 삽입에서는 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

그 외에 문자열 타입이 아닌 값을 문자열 타입으로 변환하고 싶다면 다음과 같이 해당 값에 빈 문자열을 연결하면 된다.

``` jsx
/* 숫자 타입 */
0 + ''         // "0"
-0 + ''        // "0"
1 + ''         // "1"
-1 + ''        // "-1"
NaN + ''       // "NaN" 
Infinity + ''  // "Infinity"
-Infinity + '' // "-Infinity"

/* Boolean 타입 */
true + ''  // "true"
false + '' // "false"

/* null, undefined 타입 */
null + ''      // "null"
undefined + '' // "undefined"

/* Symbol 타입 */
(Symbol()) + '' // TypeError: Cannot convert a Symbol value to a string

/* 객체 타입 */
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[12, 20] + ''       // "10,20"
(function(){}) + '' // "function(){}"
Array + ''          // "function Array() { [native code] }"
```

### 9.2.2. 숫자 타입으로 변환

자바스크립트 엔진은 산술 연산자의 피연산자 중 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다. 

``` jsx
1 - '1'   // 0
1 * '10'  // 10
1 / 'one' // NaN
```

피연산자의 크기를 비교해야 하는 비교 연산자의 경우도 피연산자를 모두 숫자 타입으로 간주한다.

``` jsx
'1' > 0 // true
```

`+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니며 숫자 타입의 값으로 암묵적 타입 변환한다.

- 빈 문자열(`''`), 빈 배열(`[]`), `null`, `false`는 0으로 변환된다.
- `true`는 1로 변환된다.
- 객체와 빈 배열이 아닌 배열, `undefined`는 변환되지 않아 `NaN`이 된다.

``` jsx
/* 문자열 타입 */
+''       // 0
+'0'      // 0
+'1'      // 1
+'string' // NaN

/* Boolean 타입 */
+true  // 1
+false // 0

/* null, undefined 타입 */
+null      // 0
+undefined // NaN

/* Symbol 타입 */
+Symbol() // TypeError: Cannot convert a Symbol value to a number

/* 객체 타입 */
+{}             // NaN
+[]             // 0
+[10, 20]       // NaN
+(function(){}) // NaN
```

### 9.2.3 불리언 타입으로 변환

자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다. 이때 값들은 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분된다.

> 즉, 참에 가까운 값과 거짓에 가까운 값

다음 값들은 모두 Falsy 값이다.

- `false`
- `undefined`
- `null`
- `0`, `-0`
- `NaN`
- `''` (빈 문자열)

Falsy 값 외의 모든 값들은 모두 `true`로 판별되는 Truthy 값이다.

``` jsx
if ('')     console.log('1');
if (true)   console.log('2');
if (0)      console.log('3');
if ('str')  console.log('4');
if (null)   console.log('5');

// 2 4
```  

## 9.3. 명시적 타입 변환

- 표준 빌트인 생성자 함수(`String`, `Number`, `Boolean`)를 `new` 연산자 없이 호출하는 방법
- 빌트인 메서드를 사용하는 방법
- 암묵적 타입 변환을 이용하는 방법

### 9.3.1. 문자열 타입으로 변환

1. `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법

``` jsx
String(1);        // "1"
String(NaN);      // "NaN"
String(Infinity); // "Infinity"
String(true);     // "true"
String(false);    // "false"
```

2. `Object.prototype.toString` 메서드를 사용하는 방법

``` jsx
(1).toString();        // "1"
(NaN).toString();      // "NaN"
(Infinity).toString(); // "Infinity"
(true).toString();     // "true"
(false).toString();    // "false"
```

3. 문자열 연결 연산자를 이용하는 방법

``` jsx
1 + '';        // "1"
NaN + '';      // "NaN"
Infinity + ''; // "Infinity"
true + '';     // "true"
false + '';    // "false"
```

### 9.3.2 숫자 타입으로 변환

1. `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법

``` jsx
Number('0');     // 0
Number('-1');    // -1
Number('10.53'); // 10.53
Number(true);    // 1
Number(false);   // 0
```

2. `parseInt`, `parseFloat` 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)

``` jsx
parseInt('0');       // 0
parseInt('-1');      // -1
parseFloat('10.53'); // 10.53
```

3. `+` 단항 산술 연산자를 이용하는 방법

``` jsx
+'0';     // 0
+'-1';    // -1
+'10.53'; // 10.53
+true;    // 1
+false;   // 0
```

4. `*` 산술 연산자를 이용하는 방법

``` jsx
'0' * 1;     // 0
'-1' * 1;    // -1
'10.53' * 1; // 10.53
true * 1;    // 1
false * 1;   // 0
```

### 9.3.3. 불리언 타입으로 변환

1. `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법

``` jsx
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true

Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true

Boolean(null);      // false
Boolean(undefined); // false

Boolean({});        // true
Boolean([]);        // true
```

2. `!` 부정 논리 연산자를 두 번 사용하는 방법

`!` 연산자는 값을 `Boolean`으로 변환하고 값을 부정한다. 그 다음 `!` 연산자를 추가적으로 호출하여 부정된 `Boolean` 값을 원래 `Boolean` 값으로 변환한다.

``` jsx
/* true 값을 부정 */
!true    // false

/* 부정된 값을 다시 복원 */
!!true   // true
!(!true) // true
```

``` jsx
!!'x';       // true
!!'';        // false
!!'false';   // true

!!0;         // false
!!1;         // true
!!NaN;       // false
!!Infinity;  // true

!!null;      // false
!!undefined; // false

!!{};        // true
!![];        // true
```

# 느낀 점 & 토론

딱 9.4절부터 이해 안되는 내용이 나오기 시작해서 내심 다행이라고 생각했다(ㅎㅎ;). 확실히 리터럴, 문자열, 표현식, 프로퍼티 등 개념이 여러 개 등장하기 시작하니 문장을 읽을 때 한 번에 와닿지 않고 여러 번 곱씹어야 했다. 개념의 중요성을 다시금 느끼는 시간이었다. 

정리하면서 자칫하면 정확히 모르고 넘어갈 수 있었던 암묵적 타입 변환에 대해 명확히 정리할 수 있었다. 부울리언 타입 변환에서 `!!` 연산자의 이용에 대해 명확히 설명되지 않아 인터넷을 찾아보았는데 그 과정에서 Truthy 값과 Falsy 값이 이곳에도 이용된다는 것을 알았다.

객체지향프로그래밍 및 실습 과목 수업을 들을 때, Shallow copy와 Deep copy에 관한 내용이 있었던 것이 기억이 난다. 당시 학기 막바지여서 수업에 집중을 하지 않을 즈음에 들은 내용이라 정확히 이해하지 못하고 넘어갔는데 이 기회에 정확히 알게 되어 좋았다.

자바스크립트 문법을 잘 이해하고 있는 개발자에게는 `(10).toString()` 보다 `10 + ''`이 더욱 간결하고 이해하기 쉽다는 대목이 있었다. 이전 시간에 느꼈던 것과 비슷하게, 자바스크립트보다 C++이나 자바를 먼저 접했던 나는 전자가 더 명확하게 의미를 전달한다고 느꼈다. 이렇게 먼저 접한 언어에 따라 편하게 느끼는 것이 다르다면, 어떤 언어를 가장 먼저 배우는 것이 여러 언어를 계속해서 배워 나가는 데에 가장 좋은 영향을 끼칠까?