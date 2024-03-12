
# Prototype

<br>

JS는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍 언어를 지원하는 멀티 패러다임 프로그래밍 언어이다.<br>
<br>

간혹 클래스와 상속, 캡슐화를 위한 키워드인 public, private 등이 없어서 객체지향 언어가 아니라고 오해하는 경우도 있다.<br>
그러나, JS는 클래스 기반 객체지향언어 보다 효율적이며 더 강력한 객체지향 능력을 지니고 있는 프로토타입 기반의 객체지향 언어이다.<br>
<br>

JS는 객체기반의 프로그래밍 언어이며, 자바스크립트를 이루고 있는 거의 모든 것이 객체다.<br>
> 원시값을 제외한 나머지 값들은 모두 객체이다.
<br>

<hr>

## 객체지향 프로그래밍

<br>

여러개의 독립적 단위 즉 객체의 집합으로 프로그램을 표현하려는 패러다임을 말한다.<br>
<br>

- 실체는 특징이나 성질을 나타내는 속성[Attribute/Property]를 가진다.<br>
- 이를 통해 실체를 인식하거나 구별한다.<br>

<hr>

### 추상화

<br>

객체의 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현한 것으로<br>
객체는 크게 두 가지로 표현할 수 있는데, <br>
 - 상태를 나타내는 프로퍼티<br>
 - 상태를 조작하는 메서드<br>
<br>
따라서 *객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.* <br>

<hr>

## 상속

<br>

- 상속을 사용하는 이유<br>
상속은 객체지향 프로그래밍의 핵심 개념으로 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것<br>
<br>

JS는 프로토타입을 기반으로 상속을 구현하여 불필요한 코드의 중복을 -> 제거한다.<br>
<br>

```JavaScript
function Circle(radius) {
  this.radius = radius;
  this.getArea = function() {
    return Math.PI * this.radius ** 2;
  }
}

const circle1 = new Circle(1); // getArea 생성
const circle2 = new Circle(2); // getArea 생성

// Circle 생성자 함수는 인스턴스를 생성할 때 마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// 때문에 동일한 메서드는 하나만 생성하여 모든 인스턴스가 공유하는 것이 좋다.
```

<br>
생성자 함수에 동일한 역할을 하는 메서드를 넣는다면 인스턴스 생성 때마다 메서드를 생성하니 메모리 측명에서 비효율적이다.<br>
<br>
이런 경우 우리는 상속을 사용할 수 있다.<br>
<br>
- 상속으로 문제 해결<br>
<br>


```JavaScript
function Circle(radius) {
  // 각각의 원 반지름 데이터(상태)는 독립접
  this.radius = radius;
}

// 같은 역할의 메서드는 Circle 객체의 프로토타입에 등록하여 공유
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// '원' 객체(인스턴스) 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);
```

위처럼 상속을 통하여 코드의 중복과 메모리 비효율을 줄일 수 있다,
<br>

<hr>

## 프로토타입 객체

<br>

JS는 프로토타입을 기반으로 상속을 구현한다.<br>
<br>
즉, 모든 객체는 자신의 프로토타입, 즉 상위[부모] 객체 역할을 하는 prototype의 모든 프로퍼티와 메서드를 상속 받는다.<br>
모든 객체는 내부슬릇으로 [[Prototype]]을 가진다.<br>
<br>

[[Prototype]] 에 저장되는 프로토타입은 객체 생성방식에 의해 결정된다.<br>
 - 객체 리터럴로 생성된 객체의 프로토타입은 = Object.prototype 을 가진다<br>
 - 생성자 함수에 의해 성성된 객체는 생성자 함수의 prototype 프로퍼티에 바인딩 된 객체를 가진다.<br>
<br>

모든 객체는 하나의 prototype을 가진다.<br>
이 말은 모든 프로토타입은 생성자 함수와 연결 되어 있다.<br>
즉, 객체 - 프로토타입 - 생성자 함수는 prototype에 의해 연결되어 있다.<br>
<br>

<hr>

## __proto__ 접근자 프로퍼티

<br>

1. __proto__는 접근자 프로퍼티다.<br>
__proto__를 통해 자신의 프로토타입 내부슬릇에 간접적으로 접근할 수 있다.<br>
즉, getter/setter 접근자 함수를 통해 취득 또는 할당할 수 있다.<br>

<br>

```JavaScript
const obj = {};
const parent = { x: 1 };

// __proto__ 접근자 프로퍼티의 getter 접근자 함수로 obj 객체의 프로토타입 객체 취득
console.log(obj.__proto__); // [Object: null prototype] {}

// __proto__ 접근자 프로퍼티의 setter 접근자 함수로 obj 객체의 프로토타입에 값 교체
obj.__proto__ = parent;
console.log(obj.__proto__); // { x: 1 }
```

<br>

2. __proto__ 접근자 프로퍼티는 상속을 통해 사용된다.<br>
__proto__는 객체가 직접 소유하는 것이 아닌 Object.prototype의 프로퍼티로 참조하고 있는 것이다.<br>
즉, 모든 객체는 상속을 통해 Object.prototype.__proto__ 를 접근자 프로퍼티로 사용할 수 있다.<br>
<br>


3. __proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유<br>
프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다. 즉, 프로토타입의 검색은 한 쪽 방향으로만 흘러야 한다.<br>
하지만 __proto__ 접근자 프로퍼티 없이 서로 상속하는 순환참조가 발생한다면 에러를 뱉지 못하겠지만 <br>
__proto__를 통해서만 접근이 가능하도록 막았기 때문에 에러를 뱉어 순환참조같은 불상사를 방지한다.<br><br>

```JavaScript
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError 에러를 뱉어준다.
```

<hr>

## 프로토타입의 constructor 프로퍼티와 생성자 함수

<br>

모든 프로토타입은 contructor 프로퍼티를 갖는다.<br>
constructor 프로퍼티는 자신을 참조하고 있는 생성자 함수를 가리킨다.<br>
리터럴 표기법으로 생성된 객체의 프로토타입의 경우, contructor가 가리키는 생성자 함수가 반드시 객체를 생성한 함수가 아닐 수 있다.

<br>
이 연결을 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.<br><br>

```JavaScript
function Person(name) {
   this.name = name;
}

const me = new Person('lee');

me.constructor === Person // true
```

<hr>

## 프로토타입의 생성시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.<br>

1. 사용자 정의 생성자 함수와 프로토타입 생성시점<br>
생서자 함수로써 호출할 수 있는 함수 즉, constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.<br>

```JavaScript
// 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입 더불어 생성된다.
// 함수 호이스팅되어 할당문 이전에 생성되어 있다.
console.log(Person.prototype); // { constructor: f }

// 생성자 함수
function Person(name) {
  this.name = name;
}

// --------------------------

const Person1 = name => {
  this.name = name;
}

console.log(Person1.prototype) // undefined로 non-constructor이기 때문에 생성X
```

2. 빌트인 생성자 함수와 프로토타입 생성시점<br>
빌트인 생성자 함수는 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성<br>
<br>
모든 빌트인 생성자 함수는 전역객체가 생성되는 시점에 생성된다.<br>
생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.<br>

<hr>

## 프로토타입 체인

프로토타입 체인에 따라 JS엔진은 프로토타입을 검색한다.<br>
객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다.<br>
따라서, 프로토타입 체인은 프로퍼티 검색과 상속을 위한다.<br>
<br>
- 스코프체인<br>
이에 반해 스코프체인은 식별자 검색을 위한 메커니즘이다. <br>
즉, 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다.<br>
<br>

> 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로피티를 검색하는데 사용된다.

<br>

***스코프체인에서 먼저 찾는다 해당 스코프에 식별자가 있는지, 없다면 프로토타입체인을 찾아서 식별자를 가져온다.***

<hr>

## 오버라이딩 / 프로퍼티 섀도잉

- 오버라이딩<br>
  상위 클래스가 가진 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.<br>
<br>

```JavaScript
const Person = (function () {
  function Person(name) { // 생성자
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi, My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("lee");
// Person.prototype 에 정의한 sayHello 프로토타입 메서드 호출
console.log(me.sayHello()); // Hi, My name is lee

// me 객체 인스턴스 메서드 정의
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
// 인스턴스 메서드 정의 이후에는 Person.prototype 에 sayHello가 아닌, 인스턴스에 정의한 sayHello를 호출
// 이것이 오버라이딩
console.log(me.sayHello()); // Hey! My name is lee
```
<br>

위 예제에서 생성자 함수 속 정의한 프로토타입 메서드를 인스턴스를 정의한 후 인스턴스 메서드로 오버라이딩 했다. <br>
즉, 프로토타입 메서드가 인스턴스 매서드에 의해 사라진 것처럼 보이는데 이를 프로퍼티 섀도잉이라 한다.<br>
<br>

> 따라서 값이 덮어 씌어진 것이 아닌 가려진것이다.

<br>
때문에, delete me.sayHello; 를 삭제하면 포로토타입 체인 상 가장 가까이 있는 인스턴스 메서드가 사라지기 때문에<br>
다시 프로토타입 메서드를 호출한다.<br>

<br>
프로토타입 메서드를 직접 삭제하려면 프로토타입에 직접 접근해야 한다.<br>
<br>

```JavaScript
delete Person.prototype.sayHello;
```

<hr>

## 프로토타입의 교체

프로토타입 즉, 부모 객체를 동적으로 변경할 수 있다.<br>
하지만 번거롭기도 하고 가독성도 안좋기 때문에 바람직하지 못하다.<br><br>

```JavaScript
1. 생성자 함수에 의한 프로토타입 교체 🚨
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정하면 파괴를 매꿀 수 있다.
    // constructor: Person,
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("lee");
// 생성자 함수에 프로퍼티로 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype 의 constructor 프로퍼티가 검색
console.log(me.constructor === Object); // true

1. 인스턴스에 의한 프로토타입 교체 🚨

function Person(name) {
  this.name = name;
}

const me = new Person("lee");

// 프로토타입으로 교환할 객체
const parent = {
  // constructor: Person,
  sayHello() {
    console.log(`Hi, My name is ${this.name}`);
  },
};

// me 객체의 프로토타입을 parent 객체로 교환한다.
Object.setPrototypeOf(me, parent);
console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

<hr>

## instanceof 연산자 - 현재 실무에서 가끔 씀

좌변에 객체를 가리키는 식별자, 우변에 생성자함수를 가리키는 식별자를 피연산자로 **우변의 피연산자가 함수가 아닌 경우 typeError발생**
우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우 false로 평가된다.<br>

```JavaScript
객체 instanceof 생성자함수

function Person(name) {
  this.name = name;
}

const me = new Person('lee');

me instnaceof Person // true
me instanceof Object // true
```

<br>
자신의 생성자 함수 뿐만 아니라 프로토타입 체인상에 있는 모든 생성자함수를 검사하여 boolean값을 뱉는다.<br>

<hr>

## 직접 상속

Object.create에 의한 직접 상속<br><br>

명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.<br>
- new 연산자 없이도 객체 생성가능<br>
- 프로토타입을 지정하면서 객체를 생성할 수 있다.<br>
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.<br>

<hr>

# 정적 프로퍼티/메서듵

정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도, 참조/호출할 수 있는 프로퍼티/메서드를 말한다.<br><br>

- 생성자 함수는 객체이므로<br>
- 자신의 프로퍼티/메서드를 소유 가능하다.<br>

```JavaScript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`HI, My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "정적 프로퍼티";

// 정적 메서드
Person.staticMethod = function () {
  console.log("정적 메서드");
};

const me = new Person("lee");

Person.staticMethod(); // 정적 메서드

me.staticMethod(); // TypeError: me.staticMethod is not a function
```

- Person 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라고 한다.<br>
- 생성자 함수안에 들어가 있는 프로퍼티/메서드가 아니기 때문에 인스턴스로 호출이 불가능하다.<br>

<hr>

## 프로퍼티 존재확인

<br>

1. in / has 연산자<br>
2. Object.prototype.hasOwnProperty<br>

```JavaScript
key in obejct // 있으면 true, 없으면 false
Reflect.has(obejct) // 있으면 true, 없으면 false

obj.hasOwnProperty('name');
```

<hr>

## 프로퍼티 열거

1. for...in 문<br>
2. Object.keys<br>
3. Object.values<br>
4. Object.entries<br>

```JavaScript
for 변수선언문 in 객체 {...}

Object.keys(obj); // 열거 가능한 키값을 배열로 반환
Object.values(obj); // 열거 가능한 값을 배열로 반환
Object.entries(obj); // 열거 가능한 키와 값의 쌍을 배열로 반환
```

