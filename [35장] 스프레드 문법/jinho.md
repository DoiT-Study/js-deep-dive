# 35장 스프레드 문법
* 스프레드 문법:  하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 **목록**으로 만든다.<br>
* 문법 적용 대상: for ... of 문으로 순회할 수 있는 이터러블에 한정 <br>
* 이터러블 예시: 배열, 문자열, map, set, DOM 컬랙션 등
```
console.log([1, 2, 3])  // 1 2 3
```
위의 예시에서 배열 [1, 2, 3]이 1 2 3으로 펼쳐진다.<br>
이때 1 2 3은 값이 아니라 목록이다 => 스프레드 문법의 결과는 변수에 할당할 수 없다. 
```
const list = [1,2,3];  // SyntaxError
```

## 35.2 배열 리터럴 내부에서 사용하는 경우
### 35.2.1 concat 
2개의 배열을 1개의 배열로 결합하고 싶은 경우 사용
```
// ES5
var arr = [1,2].concat([3,4]);
console.log(arr); // [1,2,3,4]
``` 
스프레드 문법을 사용하는 경우
```
// ES6
const arr = [...[1,2],...[3,4]];
console.log(arr);  // [1,2,3,4]
```

### 35.2.2 splice
배열의 중간에 다른 배열의 요소들을 추가하거나 제거하고 싶은 경우
```
//ES5
var arr1 = [1,4];
var arr2 = [2,3];

arr1.splice(1,0,arr2);
console.log(arr1)  // [1, [2, 3], 4]

```
배열의 요소가 추가되도록 하려면 Fuction.prototype.apply 메서드를 사용하여 splice메서드를 호출해야 한다. 

```
ES6 
const arr1 = [1,4];
const arr2 = [2,3];
arr1.splice(1,0,...arr2);
console.log(arr1); // [1, 2, 3, 4]
```
스프레드 문법을 사용할 경우 간단하게 배열의 요소 추가가능

### 35.2.3 배열 복사
```
//ES5
var origin = [1,2];
var copy = origin.slice();
```

```
///ES6
const origin = [1,2];
const copy = [...origin];

```

### 35.2.4 이터러블을 배열로 변환

메서드를 사용하여 slice 메서드를 호출해야 한다. => 이터러블뿐만 아니라 유사 배열 객체도 배열로 변환할 수 있다.<br> 

```
function sum(){
 // 이터러블이면서 유사 배열 객체인 argument를 배열로 변환
  return[...arguments].reduce((pre, cur) => pre + cur, 0);  
}
```
