
# class

JS는 프로토타입 기반의 객체지향 언어이다.
때문에 클래스 지향의 언어들과 달리 클래스가 필요하지 않았지만

클래스 기반 언어에 익숙한 개발자들에게 높은 진입장벽이었다.
따라서, 그들의 선호를 위해 클래스를 ES6 부터 추가되었다.

> JS에서 클래스는 함수로 이루어져 있다.
> 기존 프로토타입의 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있게하고 이름만 클래스랑 정의한 **문법적 설탕**

## 클래스와 생성자 함수의 차이점

- 클래스는 new 연산자 없이 호출하면 에러가 발생, 생성자 함수는 일반 함수로써 동작
- 클래스는 extends uper키워드 사용가능
- **클래스는 호이스팅이 발생하지 않는 것처럼 동작한다, 생성자 함수는 호이스팅 동작**
- 클래스 내부에서 암묵적으로 strict mode 적용

# 클래스 정의

class 키워드를 사용해 정의한다.

```JavaScript
class Person {}

// 익명 클래스
const Person = class {};

// 기명 클래스
const Person = class MyClass {};

  // 기명클래스에선 Person이 class를 가리키는 식별자이다.
  // 따라서 Person을 이용하여 인스턴스를 생성해야 한다.
  // MyClass라는 이름은 코드 내부에서만 사용가능하기 때문.
```

클래스는 내부적으로 함수이므로 당연히 일급객체로 동작한다.

> 클래스가 가지는 일급객체의 특징
> - 무명리터럴로 생성 가능, 즉, 런타임 생성가능
> - 변수나 자료구조에 저장 가능
> - 함수의 매개변수에게 전달 가능
> - 함수의 반환값으로 사용 가능

## 클래스와 생성자 함수의 정의방식

```JavaScript
// 클래스 정의
class Person = {

// constructor
  constructor(name) {
    this.name = name;
  }

// 프로토타입 메서드
  sayHI() {
    console.log(`${this.name}`);
  }

// 정적 메서드
  static sayHello() {
    console.log('hello');
  }
}

// +++++++++++++++++++++++++++++

// 생성자 함수 정의
var Person = (function() {

// constructor
  function Person() {
    this.name = name;
  }

// 프로토타입 메서드
  Person.prototype.sayHi = function() {
    console.log('hi');
  }

// 정적 메서드
  Person.sayHello = function() {
    console.log('hello');
  }
}())
```


# 클래스 호이스팅

```JavaScript
class Person {}

console.log(typeof Person) // function

// +++++++++++++++++++++++++++++

console.log(typeof Person) // ReferenceError

class Person {}
```

클래스 선언문으로 생성된 클래스는 
실행단계[런타임] 이전 평가단계에서 평가되어 함수 객체[constructor생성자함수]를 생성한다.

클래스는 let, const와 같이 호이스팅이 동작하지 않는 것처럼 보여진다.
마찬가지로 TDZ에 빠지게 되는 것


## 클래스 정의 가능한 메서드

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다. 
`constructor`, `프로토타입 메서드`, `정적 메서드`

```JavaScript
class Person = {

// constructor
  constructor(name) {
    this.name = name;
  }

// 프로토타입 메서드
  sayHI() {
    console.log(`${this.name}`);
  }

// 정적 메서드
  static sayHello() {
    console.log('hello');
  }
}

cosnt me = new Person('LEE');

// 인스턴스 프로퍼티 참조
console.log(me.name);

// 프로토타입 메서드 호출
me.sayHI(); // LEE

// 정적 메서드 호출
Person.sayHello(); // hello
```

1. constructor 생성자함수
   인스턴스를 초기화하고 초기화한다.

   - constructor는 한개만 존재 가능
   - 생략 시 암묵적으로 빈 constructor생성
   - 인스턴스를 초기화하려면 필수
   - 별도의 반환문 X -> 암묵적으로 해당 인스턴스[this] 반환

2. 프로토타입 메서드
   클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입에 속해져 프로토타입 메서드가 된다.

3. 정적 메서드
   ****정적 메서드는 인스턴스 없이 호출할 수 있는 메서드를 의미한다.****

   - 클래스에서는 static 키워드를 이용해 생성한다.
   - 인스턴스로는 호출할 수 없고, 클래스명.함수명() 으로만 호출이 가능하다.

> 정적메서드 vs 프로토타입메서드
>
>> 서로 속해 있는 프로토타입의 체인이 다르다.
>> 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
>> 따라서, 정적메서드는 인스턴스 프로퍼티를 참조 할 수 없지만,
>> 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

> 클래스에서 정의한 메서드의 특징
>
> - function 키워드 생략한 축약 표현 사용가능
> - for...in Object.keys등으로 열거 불가능 [[Enumerable]]의 값이 false이기 때문

# 클래스의 인스턴스 생성과정[요약]

 1. new 연산자와 클래스 호출 시 내부 [[Constructor]]가 호출되고 암묵적 빈 객체 생성
 2. 인스턴스 생성 this 바인딩
 3. this에 바인딩 된 consturctor를 통해 인스턴스 초기화  [없다면 생략]
 4. 인스턴스 즉, 객체에 바인딩 된 this를 반환


## private, static 필드 정의

js는 접근제한자를 지원하지 않기 때문에 인스턴스 프로퍼티는 언제나 public하다.

그러나 private 필드 정의할 수 있는 사양이 제안되었다.

```JavaScript
class Person {
  #name = '';
  static num = 1;

  constructor(name) {
    this.name = name;
  }
}

const me = new Person();
console.log(me.#name); // SyntaxError

console.log(Person.num); // 1 정적 필드 추가
```

private 필드는 # 을 붙여준다. 참조할 때도 마찬가지

이로써 private는 클래스 내부에서만 참조가 가능하다.
반드시 생성자 내부가 아닌 몸체에 정의해야 하고,

보통 getter, setter 메서드를 정의하여 같이 사용한다.



# 상속에 의한 클래스 확장

프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 것이다.
상속에 의한 클래스 확장은 기존 클래스를 상속 받아 부모 클래스의 자산을 얻고, 자식 클래스에서 확장하거나 재정의하여 사용하는 것이다.

- ***extends 키워드***
  우측에 클래스를 좌측 클래스로 상속 시켜주는 키워드로
  왼쪽이 부모, 오른쪽이 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식이다.

  ```JavaScript
  function Base(a) {
    this.a = a;
  }

  class Child extends Base {}

  const child = new Child(4);
  console.log(child) // Child { a : 1 }
  ```

   - 서브클래스의 constructor

     ```JavaScript
       // 서브 클래스 생성자 constructor
       constructor(...args) { super(...args); }
     ```

     클래스에서 constructor를 생략하면 비어있는 constructorrk 암묵적으로 정의된다.
     서브클래스에서 constructor를 생략하면 위 예제와 같이
     super키워드를 사용해 부모클래스의 constructor를 호출하여 인스턴스를 생성한다.
     

- ***super 키워드***
  super키워드는 함수처럼 호출이 가능하고, this와 같이 식별자로도 사용이 가능하다.

  super를 호출하여 슈퍼 클래스의 constructor를 호출한다.
  super를 참조하면, 슈퍼 클래스의 메서드를 호출가능하다.
  
  - super호출
    super를 호출하면 슈퍼클래스의 constructor를 호출한다.

    ```JavaScript
    class Base(a) {
      constructor(a) {
        this.a  =  a;
      }
    }

    class Child extends Base {
      // 암묵적으로 super constructor 정의
    }

    const child = new Child(5);
    console.log(child.a) // 5;
    ```
    - 주의사항
    서브클래스에서 constructor 정의 시 super 키워드 필수
    서브클래스에서 super참조 전까지 this사용 불가
    서브클래스에서만 호출하도록 한다.
    
  - super참조
    메서드 내에서 super를 참조하면 슈퍼클래스의 메서드를 호출 할 수 있다.

    ```JavaScript
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHI() {
        return `HI, ${this.name}`;
      }
    }

    class Child extends Base {
      sayHI() {
        return `$(super.sayHI()) ^^`;
      }
    }

    const child = new Child("LEE");
    console.log(child.sayHI()); // HI LEE ^^
    ```

    위 예제에서 super.sayHI 메서드는 슈퍼클래스의 프로토타입 메서드를 가리킨다.
    































