# 24장 클로저

## 24.1 렉시컬 스코프 (정적 스코프)
#### ? 렉시컬 스코프
함수를 어디에 정의했는지에 따라 결정된 상위 스코프

<br>

상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경에 의해 결정됨. => 렉스컬 스코프 개념이 클로저에서 중요한 이유

<br>

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

#### ? 내부 슬롯 [[Environment]]
함수가 자신이 정의된 환경(상위 스코프)의 참조를 저장하기 위해 사용되는 함수의 내부 슬롯

<br>

함수의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조 = 상위 스코프 

<br>

## 24.3 클로저와 렉시컬 환경

#### ? 클로저 
외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있는 중첩 함수

#### ? 자유 변수
클로저에 의해 참조되는 상위 스코프의 변수

<br>

```javascript
const x = 1;

function outer() {
    const x = 10;   // 자유 변수
    const inner = function() { console.log(x); };
    return inner;
}

const innerFunc = outer();
innerFunc();    // 10
```
? 어떻게 "innerFunc();" 의 결과가 10일까
<br>
중첩 함수 inner의 내부 슬롯 [[Environment]]에 outer 함수의 렉시컬 환경을 상위 스코프로서 저장 -> outer 함수는 실행 종료 이후에도 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있으므로 렉시컬 환경이 유지됨 -> 중첩 함수는 상위 스코프를 참조할 수 있으므로 inner 함수는 상위 스코프인 outer 함수의 변수에 접근이 가능함

<br>

## 24.4 클로저의 활용

#### ? 클로저의 사용 의도
상태를 안전하게 변경하고 유지하기 위해 사용
-> 쉽게 접근하여 상태를 변경하는 상황을 방지하기 위해 사용

<br>

```javascript
const increase = (function () {
    let num = 0;

    return function() {
        return ++num;
    };
}());

console.log(increase());    // 1
console.log(increase());    // 2
```
상태가 노출되어 의도치 않게 변경되는 상황을 막기 위해 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용

<br>

## 24.5 캡슐화와 정보 은닉

#### ? 캡슐화
객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것

<br>

#### ? 정보 은닉
객체의 특정 프로퍼티나 메서드를 감추는 것

<br>

#### 스코프 활용

<br>

## 24.6 자주 발생하는 실수

스코프를 고려하지 않고 코드를 작성한 경우
<br>

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = function() { return i; };
}

for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
```

<br>

스코프를 고려하여 코드를 작성한 경우
<br>

1) 클로저를 사용

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = (function (id) {
        return function () {
            return id;
        };
    }(id));
}

for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
```

<br>

2) 지역 변수 let 사용
<br>

```javascript
var funcs = [];

for (let i = 0; i < 3; i++) {
    funcs[i] = function() { return i; };
}

for (let j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
```
