
# 객체 리터럴

<hr />

## 객체

JS는 `객체 기반`의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 `모든 것은 객체`이다.<br />
원시값을 제외한 나머지 값은 모두 객체다 (`함수`, `배열`, `정규표현식` 등)<br />
<br />
객체타입은 다양한 타입의 값을 하나의 단위로 구성한 `복합적인 자료구조`다,<br />
또한 원시값은 변경 불가능한 값으로 항상 새로운 값을 만들고 새로 할당했지만<br />
`객체는 변경 가능한 값`이다.<br />
즉, 객체는 기존 프로퍼티 값을 덮어쓸 수 있다.<br />
<br />
> 객체의 구성
> ```JavaScript
>  var person = {
>   name : 'leesoobo'
>  }
> ```
<br />

자바스크립트에서 사용할 수 있는 `모든 값`은 객체의 `프로퍼티`가 될 수 있다.<br />
JS의 함수는 일급객체이기 때문에 당연하게도 객체의 프로퍼티로 사용될 수 있고, 이때 함수를 `메서드`라고 부른다.<br />
<br />

- 프로퍼티 : 객체의 상태를 나타내는 값<br />
- 메서드 : 프로퍼티를 참조하고 조작할 수 있는 동작

<hr />

## 객체 리터럴에 의한 객체 생성

자바스크립트의 객체는 다양한 객체 생성 방법을 지원한다.
- `객체 리터럴`
- `Object 생성자 함수`
- `생성자 함수`
- `Object.create메서드`
- `클래스 ES6`

이러한 방법 중 가장 많이 사용되고 간단한 방법은 아래 예제의 `객체 리터럴`이다.
```JavaScript
// 객체 리터럴
  var person = {
   name : 'leesoobo'
  }
// 변수에 할당 되는 시점에 객체가 생성된다.
```

<hr />

## 객체 사용법
```JavaScript
 // 객체 리터럴 생성
 var person = {
  nm : 'lee'
 }

 // 겍체 값 접근
 console.log(person.nm);

 // 프로퍼티 갱신
 person.nm = "kim";

 // 프로퍼티 추가
 person.age = '29';

 // 프로퍼티 삭제
 delete person.age
```

<hr />

## 프로퍼티 축약표현
```JavaScript
 var x = 1;
 var y = 2;

 var person = {
  x, y
 }

 console.log(person); // { x : 1, y : 2}
```

<hr />

## 메서드 축약표현
```JavaScript
var obj = {
 name : 'Lee',
  sayHi : function() {
   console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee

// 메서드 축약하기
var obj = {
 name : 'Lee',
  sayHi() {
   console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```
