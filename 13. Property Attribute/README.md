
# Propery Attribute

<br>

JS 엔진은 프로퍼티를 생성할 때 프로퍼티 상태를 기본값으로 자동 정의한다.<br>
<br>

프로퍼티 상태<br>
- 프로퍼티의 값[value] <br>
- 값의 갱신가능 여부[writable] <br>
- 열거가능 여부[enumerable] <br>
- 재정의 가능 여부[configurable] <br>
<br>

> 다음과 같은 상태들에는 엔진이 관리하기 때문에 직접적으로 접근할 수 는 없다. 우리는 Object.getOwnPropertyDescriptor 메서드를 > 이용해 간접적으로 접근가능하다.

<br>

## 내부 슬릇 & 내부 메서드

<br>

내부 슬릇 & 내부 메서드는 JS엔진 안에서 실제로 동작은 하지만 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼틴 아니다.<br>
따라서, 직접 접근하거나 호출할 수 있는 방법을 제공하지는 않는다.<br>
단, 일부 내부 슬룻 & 내부 메서드에 접근할 수 있는 수단을 제공한다.<br>
 - 예를 들어 [[Prototype]] 내부 슬릇은 __proto__를 통해 접근 가능하다.<br>

<br>

## 프로퍼티 [데이터 & 접근자]

<br>

객체의 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.<br>
<br>

- 데이터 프로퍼티<br>
  키와 값으로 구성된 일반적인 프로퍼티다.<br>
<br>
  
[[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 로 구성<br>

<br>
프로퍼티 생성 시 <br>
[[Value]] 값은 프로퍼티 값으로 초기화<br>
나머지는 모두 true로 기본값 초기화<br>
<br>


```JavaScript
const person = {
  name ; 'kim'
}

// 프로퍼티 동적 생성
person.age = 29;

console.log(Object.getOwnPropertyDescriptors(person));

// {
//   name: { value: 'WI', writable: true, enumerable: true, configurable: true },
//   age: { value: 100, writable: true, enumerable: true, configurable: true }
// }
```

<br>

- 접근자 프로퍼티<br>
  자체적으로는 값을 갖지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.<br>
<br>
  
[[Get]], [[Set]], [[Enumerable]], [[Configurable]] 로 구성<br>
데이터 프로퍼티와 Enumerable, Configurable 은 같은 Attribute이다.<br>

<br>
접근자 함수는 말그대로 getter / setter 함수라고도 한다.
<br>

```JavaScript
const person = {
  firstName: "Youngmin",
  lastName: "WI",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + " " + person.lastName); // Youngmin WI

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장 [setter 호출] 
person.fullName = "Youngmaaan WIE";
console.log(person); // { firstName: 'Youngmaaan', lastName: 'WIE', fullName: [Getter/Setter] }

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조 [getter 호출]
console.log(person.fullName); // Youngmaaan WIE

// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
// 현 코드에서 "firstName" 프로퍼티는 "데이터 프로퍼티"다.
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
/*
{
  value: 'Youngmaaan',
  writable: true,
  enumerable: true,
  configurable: true
} 
*/

// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
// 현 코드에서 "fullName" 프로퍼티는 "접근자 프로퍼티"다.
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
/*
{
  get: [Function: get fullName],
  set: [Function: set fullName],
  enumerable: true,
  configurable: true
}
*/
```

<br>

## 객체 변경 방지

<br>

객체는 기본적으로 변경 가능한 값이라고 했다.<br>
즉, 재할당 없지 직접 변경 가능함을 의미한다.<br>
<br>
하지만 때에 따라 객체를 변경하면 안되는 경우가 존재하고 이를 지원하는 메서드가 존재한다.<br>
<br>

1. Object,preventExtensions [객체 확장금지]<br>
   추가 : X<br>
   삭제 : O<br>
   읽기 : O<br>
   쓰기 : O<br>
   재정의 : O<br>
<br>

2. Object.isSealed [객체 밀봉]<br>
   추가 : X<br>
   삭제 : X<br>
   읽기 : O<br>
   쓰기 : O<br>
   재정의 : X<br>
<br>

3. Object,freeze [객체 동결]<br>
   추가 : X<br>
   삭제 : X<br>
   읽기 : O<br>
   쓰기 : X<br>
   재정의 : X<br>

