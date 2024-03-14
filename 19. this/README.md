
# this 키워드

객체는 상태와 동작 즉, 프로퍼티와 메서드로 묶은 자료구조이고<br>
이 자료구조에 메서드가 접근하여 프로퍼티를 관리하고 변경할 수 있어야 한다.<br><br>

이 때 메서드는 자기 자신이 속한 객체를 가리켜야 한다.<br><br>

즉, `this`는 자신이 속한 객체 또는 자신이 생성한 인스턴스를 가리키는 ***자기 참조 변수*** 이다.<br>



### 특징

- this는 자바스크립트 엔진에 의해 `암묵적으로 생성`된다.<br>
- this는 `함수 호출 방식`에 의해 `this바인딩이 결정`된다.<br><br>

```JavaScript
// 객체 리터럴
const circle = {
  radius: 5,

  getDiameter() {
    // this는 메서드를 호출한 객체
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter());  // 10
```
위 예제에서는 circle를 직접적으로 가리킨다.<br><br>


```JavaScript
// 생성자 함수
function Circle(radius) {
  // 여기서 this는 생성자 함수 Circle이 생성할 인스턴스
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 여기서 this는 생성자 함수가 생성할 인스턴스
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());  // 10
```
위 예제에서는 생성자 함수가 실행되며 생성할 인스턴스를 가리킨다.<br><br>

자바나 C++ 같은 클래스 기반 언어는 언제나 this가 자신 클래스가 생성하는 인스턴스를 가리키지만<br>
JS에서는 this바인딩이 `함수 호출방식`에 따라 `동적`으로 바인딩 된다.<br><br>

1. `전역에서의 this`<br>
  - 전역에서 this는 `전역객체`를 가리킨다.<br><br>
    
2. `일반함수에서 this`<br>
  - 일반함수 객체 내부에서는 `언제나 전역객체` window를 가리킨다.<br>
  - `중첩함수`도 일반함수라면 전역객체를 가리킨다.<br>
  - `콜백함수`로 들어온 내부 this도 전역객체를 가리킨다.<br><br>

this 명시적 바인딩
```JavaScript
const obj = {
  value = 100;
  foo() {
    const that = this; // 메서드 this는 obj를 가르킴

    setTimeout(function(){
      console.log(that.value); // 100
    }, 100)
  }
}
```

<br>

3. `메서드에서 this`<br>
  - 메서드 내부에서 this는 `메서드를 호출한 객체`를 가리킨다. [위에서 첫 예제]<br>
  - 즉, 메서드를 호출할 때 메서드 앞에 마침표[.] 연산자 앞에 기술한 객체가 바인딩된다.<br><br>

위 특징 예제
```JavaScript
const person = {
  name: "LEE",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    return this.name;
  },
};

console.log(person.getName());  //  LEE person객체에서 호출했기 때문

// 다른 객체 선언
const anotherPerson = {
  name: "KIM",
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName을 호출한 객체는 이 시점에선 person이 아닌 anotherPerson이므로 this는 후자를 가르킨다.
console.log(anotherPerson.getName());  // KIM

// getName을 getName 변수에 할당 일반 변수에서는 전역객체 가리킴
const getName = person.getName;

// getName을 호출한 객체는 이 시점에서는 전역 객체다.
// 전역 객체에 프로퍼티에는 name 이라는 프로퍼티가 존재하지 않다.
// 참조 시, 자바스크립트 엔진이 암묵적으로 undefined 로 초기화한다.
console.log(getName());  // undefined  -> window.name과 같다.
```

<br>

4. `생성자함수 this`<br>
  - 생성자 함수 내부에서 this는 생성자 함수가 `생성할 인스턴스`를 가리킨다. [위에서 두번째 예제]<br><br>

```JavaScript
// 생성자 함수
function Person(name) {
  this.name = name;
  this.getName = function () {
    return `안녕 나는 ${this.name}야.`;
  };
}

const person1 = new Person("LEESOO");
const person2 = new Person("LEESOOBO");

console.log(person1.getName()); // 안녕 나는 LEESOO야.   -> person1을 가르킴
console.log(person2.getName()); // 안녕 나는 LEESOOBO야. -> person2을 가르킴

// 하지만 생성자 함수를 new 없이 호출하면 일반 함수로써 호출
const person3 = Person("LEE");

console.log(person3) // undefined  // return 없으니 암묵적으로 undefined 반환
console.log(name) // LEE -> window.name과 같음 -> name은 암묵적 전역에 의해 window프로퍼티가 되었음
```

<br>

## Function.prototype,apply / call / bind 메서드로 간접호출

위 메서드는 this로 사용할 객체와 인수 리스트를 매개로 받아 함수를 호출한다.<br>

```JavaScript
function returnObj () {
  return this;  
}

const thisObj = { a : 1 };

console.log(returnObj()); // window

console.log(returnObj.apply(thisObj)); // { a : 1 }
console.log(returnObj.call(thisObj));  // { a : 1 }
```

<br>

다음과 같이 함수에서 사용할 this를 전달하여 this 바인딩이 가능하다.<br><br>

유사 배열 객체를 인수로 집어 넣어 `배열로 반환해` 사용하는데 사용하기도 한다.<br><br>

- bind 메서드<br><br>

```JavaScript
function returnObj() {
  return this;
}

const thisObj = { a : 1 }

console.log(returnObj.bind(thisObj)); // F returnObj
console.log(returnObj.bind(thisObj))(); // { a : 1 }

// bind 메서드는 this가 교체된 함수를 반환하므로 함수를 호출해줘야 진짜 의미가 있다.
```

함수를 반환하는 특징은 `콜백 함수`에서 this를 정의할 때 많이 사용한다.<br><br>

- 콜백함수 바인딩 해결
```JavaScript
const person = {
  name: "LEE",
  foo(callback) {
    // bind함수로 메서드 내부 this 바인딩 [ 3 ]
    setTimeout(callback.bind(this), 100);
  },
};

// 메서드 호출 메서드는 person = this [ 1 ]
person.foo(function () {
  // 일반 함수로써 호출 일반함수는 window = this [ 2 ]
  console.log(`안녕하세요. ${this.name}입니다.`); // 안녕하세요. LEE입니다.
});
```


















