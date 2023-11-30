# 35. 스프레드 문법

스프레드 문법 (전개 문법) `...`는 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐 개별적인 값들의 목록으로 만든다. 스프레드 문법을 사용할 수 있는 대상은 `Array`, `String`, `Map`, `Set`, DOM 컬렉션(`NodeList`, `HTMLCollection`), `arguments`와 같이 `for...of` 문으로 순회할 수 있는 **이터러블**에 한정된다. 스프레드 문법의 결과는 값이 아니라 값들의 나열이다. 따라서 변수에 할당할 수 없다.

```jsx
console.log(...[1, 2, 3]); // 1 2 3

console.log(...'Hello'); // H e l l o

console.log(...new Map([['a', '1'], ['b', '2']])); // ['a', '1'] ['b', '2']
console.log(...new Set([1, 2, 3])); // 1 2 3

/*이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다*/
console.log(...{ a: 1, b: 2 }); // TypeError: Found non-callable @@iterator

/* 변수에 할당 불가 */
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token ...
```

## 35.1. 함수 호출문의 인수 목록에서 사용하는 경우

`Math.max` 메서드를 사용할 때, 배열을 펼쳐 요소들을 개별적인 값들의 목록으로 만든 후 인수로 전달해야 한다.

```jsx
const arr = [1, 2, 3];

const max = Math.max(...arr); // 3
```

이때 Rest 파라미터와 형태가 동일하여 혼동할 수 있다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 `...`을 붙이는 것이다. 따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

## 35.2. 배열 리터럴 내부에서 사용하는 경우

### 35.2.1. `concat`

ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 `concat` 메서드를 사용해야 했다. 스프레드 문법을 사용하면 별도의 메서드를 사용하지 않아도 된다.

```jsx
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### 35.2.2. `splice`

ES5에서 어떤 배열의 중간에 다른 배열의 요소를 추가하거나 제거하려면 `Function.Prototype.apply` 메서드를 사용하여 `splice` 메서드를 호출해야 한다. 스프레드 문법을 사용하면 `splice` 메서드 하나만 사용해도 된다.

```jsx
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

### 35.2.3. 배열 복사

스프레드 문법을 사용하면 배열을 복사할 수 있다. 이때 `shallow copy`가 이루어진다.

```jsx
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

### 35.2.4. 이터러블을 배열로 변환

`arguments` 객체는 이터러블이면서 유사 배열 객체이기에 스프레드 문법을 사용할 수 있다.

```jsx
function sum() {
    return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

그러나 이터러블이 아닌 유사 배열 객체에는 스프레드 문법을 사용할 수 없다. 이를 위해서는 `Array.from` 메서드를 사용한다.

```jsx
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

const arr = [...arrayLike]; // TypeError: object is not iterable

Array.from(arrayLike); // [1, 2, 3]
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

```jsx
const obj = { x: 1, y: 2 };
const copy = { ...obj };

console.log(copy); // { x: 1, y: 2 }
console.log(obj == copy); // false
```

```jsx
const merged = {x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가할 수 있다.

```jsx
/* 객체 병합: 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다 */
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3}};
console.log(merged); // { x: 1, y: 10, z: 3 }

/* 특정 프로퍼티 변경 */
const changed = { ...{ x: 1, y: 2 }, y: 100 };
console.log(changed); // { x: 1, y: 100 }

/* 프로퍼티 추가 */
const added = { ...{ x: 1, y: 2 }, z: 0 };
console.log(added); // { x: 1, y: 2, z: 0 }
```
