# 11.2 객체

## 객체
- 프로퍼티의 개수가 정해져 있지 않으며 동적으로 추가/삭제 가능
- 사전에 확보해야 할 메모리 공간의 크기 설정 불가능


## 11.2.1 변경 가능한 값

원시 값 할당한 변수의 메모리 주소 -> 참조 값 -> 원시 값
<br>
*참조값 : 생성된 객체가 저장된 메모리 공간의 주소
<br>
### 예제1)

    var person = {
        name: 'Lee'
    };

    console.log(person);    //{name: "Lee"}

<br>
객체를 할당한 변수는 재할당 없이 객체를 직접 변경 가능
<br>
재할당 없이 프로퍼티 값 수정/추가/삭제 가능

### 예제2)

    var person = {
        name: 'Lee'
    };

    console.log(person);    //{name: "Lee"}

    person.name = "Jung";

    console.log(person);    //{name: "Jung"}



## 11.2.2 참조에 의한 전달

참조에 의한 전달
<br>
: 원본의 참조 값이 복사되어 전달되는 방식

### 예제)

    var person = {
        name: 'Lee'
    };

    var copy = person;

    console.log(copy);  //{name: "Lee"}

- 원본 person을 사본 copy에 할당하면 원본 person의 참조 값을 복사해서 copy에 저장
- 다른 메모리 주소값을 가지지만 동일한 참조 값을 가짐
<br>
=> 두 개의 식별자(person, copy)가 하나의 객체({name: "Lee"})를 공유함
