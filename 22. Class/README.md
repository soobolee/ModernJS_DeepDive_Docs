
# class

JS는 `프로토타입 기반`의 객체지향 언어이다.<br>
때문에 클래스 지향의 언어들과 달리 클래스가 필요하지 않았지만<br><br>

클래스 기반 언어에 익숙한 개발자들에게 높은 진입장벽이었다.<br>
따라서, 그들의 선호를 위해 `클래스`를 `ES6 부터 추가`되었다.<br><br>

> JS에서 클래스는 `함수`로 이루어져 있다.<br>
> 기존 프로토타입의 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있게하고 이름만 클래스랑 정의한 **문법적 설탕**<br>

## 클래스와 생성자 함수의 차이점

- 클래스는 new 연산자 없이 호출하면 `에러`가 발생, 생성자 함수는 일반 함수로써 동작<br>
- 클래스는 `extends` `super`키워드 사용가능<br>
- **클래스는 호이스팅이 발생하지 `않는 것`처럼 `동작`한다, 생성자 함수는 호이스팅 동작**<br>
- 클래스 내부에서 암묵적으로 `strict mode` 적용<br><br>

# 클래스 정의

class 키워드를 사용해 정의한다.<br><br>

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

클래스는 내부적으로 함수이므로 당연히 `일급객체`로 동작한다.<br><br>

> 클래스가 가지는 일급객체의 `특징`<br>
> - 무명리터럴로 생성 가능, 즉, `런타임 생성가능`<br>
> - 변수나 자료구조에 `저장` 가능<br>
> - 함수의 매개변수에게 `전달` 가능<br>
> - 함수의 반환값으로 `사용` 가능<br><br>

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

<br>

# 클래스 호이스팅

```JavaScript
class Person {}

console.log(typeof Person) // function

// +++++++++++++++++++++++++++++

console.log(typeof Person) // ReferenceError

class Person {}
```

클래스 선언문으로 생성된 클래스는 <br>
실행단계[런타임] 이전 평가단계에서 평가되어 함수 객체[constructor생성자함수]를 생성한다.<br><br>

클래스는 `let`, `const`와 같이 호이스팅이 동작하지 않는 것처럼 보여진다.<br>
마찬가지로 `TDZ`에 빠지게 되는 것<br><br>


## 클래스 정의 가능한 메서드

클래스 몸체에는 `0개 이상`의 `메서드`만 정의할 수 있다. <br>
`constructor`, `프로토타입 메서드`, `정적 메서드` <br><br>

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

1. constructor 생성자함수<br>
   `인스턴스`를 `바인딩`하고 `초기화`한다.<br><br>

   - constructor는 `한개만` 존재 가능<br>
   - 생략 시 암묵적으로 `빈 constructor생성`<br>
   - 인스턴스를 초기화하려면 `필수`<br>
   - 별도의 반환문 X -> 암묵적으로 해당 `인스턴스[this] 반환`<br><br>

2. 프로토타입 메서드<br>
   클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입에 속해져 `프로토타입 메서드`가 된다.<br><br>

3. 정적 메서드<br>
   ****정적 메서드는 인스턴스 없이 호출할 수 있는 메서드를 의미한다.**** <br><br>

   - 클래스에서는 `static` 키워드를 이용해 생성한다.<br>
   - 인스턴스로는 호출할 수 없고, `클래스명.함수명()` 으로만 `호출이 가능`하다.<br><br>

> 정적메서드 vs 프로토타입메서드<br>
><br>
>> 서로 속해 있는 `프로토타입의 체인`이 `다르다.`<br>
>> 정적 메서드는 `클래스로 호출`하고 프로토타입 메서드는 `인스턴스로 호출`한다.<br>
>> 따라서, 정적메서드는 인스턴스 프로퍼티를 참조 할 수 `없지`만,<br>
>> 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 `있다`.<br>

<br>

> 클래스에서 정의한 메서드의 특징<br>
><br>
> - function 키워드 생략한 축약 표현 사용가능<br>
> - for...in Object.keys등으로 `열거 불가능` -> [[Enumerable]]의 값이 false이기 때문<br><br>

# 클래스의 인스턴스 생성과정[요약]

<br>

 1. new 연산자와 클래스 호출 시 내부 [[Constructor]]가 호출되고 암묵적 빈 객체 생성<br>
 2. 인스턴스 생성 this 바인딩<br>
 3. this에 바인딩 된 consturctor를 통해 인스턴스 초기화  [없다면 생략]<br>
 4. 인스턴스 즉, 객체에 바인딩 된 this를 반환<br><br>


## private, static 필드 정의

js는 접근제한자를 지원하지 않기 때문에 인스턴스 프로퍼티는 언제나 public하다.<br><br>

그러나 private 필드 정의할 수 있는 사양이 제안되었다.<br>

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

`private 필드`는 `#` 을 붙여준다. 참조할 때도 마찬가지<br><br>

이로써 private는 클래스 내부에서만 참조가 가능하다.<br>
반드시 생성자 내부가 아닌 몸체에 정의해야 하고,<br><br>

보통 `getter`, `setter` 메서드를 정의하여 같이 사용한다.<br><br>



# 상속에 의한 클래스 확장

프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 것이다.<br>
상속에 의한 클래스 확장은 기존 클래스를 상속 받아 부모 클래스의 자산을 얻고, 자식 클래스에서 확장하거나 재정의하여 사용하는 것이다.<br><br>

- ***extends 키워드*** <br>
  우측에 클래스를 좌측 클래스로 상속 시켜주는 키워드로<br>
  왼쪽이 `부모`, 오른쪽이 `[[Construct]] 내부 메서드를 갖는 함수 객체`로 평가될 수 있는 모든 표현식이다.<br><br>

  ```JavaScript
  function Base(a) {
    this.a = a;
  }

  class Child extends Base {}

  const child = new Child(4);
  console.log(child) // Child { a : 1 }
  ```

   - 서브클래스의 constructor<br>

     ```JavaScript
       // 서브 클래스 생성자 constructor
       constructor(...args) { super(...args); }
     ```

     클래스에서 constructor를 생략하면 비어있는 constructor `암묵적`으로 정의된다.<br>
     서브클래스에서 constructor를 생략하면 위 예제와 같이<br>
     super키워드를 사용해 부모클래스의 constructor를 호출하여 인스턴스를 생성한다.<br>
     

- ***super 키워드*** <br>
  super키워드는 함수처럼 `호출`이 가능하고, this와 같이 `식별자`로도 사용이 가능하다.<br><br>

  super를 `호출` -> 하여 슈퍼 클래스의 constructor를 호출한다.<br>
  super를 `참조` -> 하면, 슈퍼 클래스의 메서드를 호출가능하다.<br>
  
  - super호출<br>
    super를 호출하면 슈퍼클래스의 constructor를 호출한다.<br><br>

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
    - `주의사항`<br>
    서브클래스에서 constructor 정의 시 super 키워드 필수<br>
    서브클래스에서 super참조 전까지 this사용 불가<br>
    서브클래스에서만 호출하도록 한다.<br>
    
  - super참조<br>
    메서드 내에서 super를 참조하면 슈퍼클래스의 메서드를 호출 할 수 있다.<br><br>

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

    위 예제에서 `super.sayHI` 메서드는 슈퍼클래스의 프로토타입 메서드를 가리킨다.<br><br>
    

## 서브클래스스의 인스턴스 생성과정 요약[요약] - 상세[책460P]

```JavaScript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  getArea() {
    return this.width * this.height;
  }
  
  toString() {
    return `width = ${this.width}, height = ${this.height}`
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }
  
  toString() {
    return `${super.toString}, color = ${this.color}`
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red')
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: 'red'}

// 상속을 통해 getArea 메서드 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

<hr>

- new 호출 시 서브클래스의 인스턴스 생성과정<br>


1. 서브클래스의 super 호출<br>
  > 자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기 위해 "base" 또는 "derived"를 값으로 갖는 내부 슬롯 [[ConstructorKind]]를 갖는다.<br>
  > 
  > 이를 통해 서브클래스로 구분된 클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다.<br>
  > 이것이 바로 서브클래스의 constructor에서 `반드시 super를 호출해야 하는 이유`다.<br>
  > 
  > 생략 시 암묵적으로 실행된다. constructor를 생략하지 않았을 때는 반드시 super를 기술해야한다.<br>
  > 실제로 인스턴스를 생성하는 주체는 수퍼클래스이므로 수퍼클래스의 constructor를 호출하는 super가 호출되지 않으면 `인스턴스를 생성할 수 없기 때문`이다.<br><br>

2. 수퍼클래스의 인스턴스 생성과 this 바인딩<br>
  > 수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다. 이 `빈 객체`가 바로 클래스가 `생성한 인스턴스`다.<br>
  > 그리고 이 인스턴스는 `this에 바인딩`된다.<br>
  > 따라서 수퍼클래스 constructor 내부의 this는 인스턴스를 가리킨다.<br>
  >       
  > 분명 인스턴스는 수퍼클래스가 생성하는 것이라고 기술했다.<br>
  > 하지만 new 연산자와 함께 호출된 클래스는 서브클래스다.<br>
  > 따라서 `new.target`은 `서브클래스`를 `가리`킨다.<br>
  > new.target은 new 연산자와 함께 호출된 함수를 가리키기 때문이다.<br>
  > 따라서 인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.<br><br>
  > 
  > 따라서 생성된 인스턴스의 프로토타입은 수퍼클래스의 prototype 프로퍼티가 가리키는 객체가 아니라 `서브클래스의 prototype 프로퍼티가 가리키는 객체다.`<br><br>

3. 수퍼클래스의 인스턴스 초기화<br>
  > 수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 `인스턴스를 초기화`한다.<br><br>

4. 서브클래스 constructor로의 복귀와 this 바인딩<br>
  > super의 호출이 종료되고 제어 흐름이 서브클래스 constructor로 돌아온다.<br>
  > 이때 super가 반환한 `인스턴스가 this에 바인딩`된다.<br>
  > 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.<br>
  > 
  > 따라서, super가 호출되지 않으면 서브클래스 constructor에서 this가 생성되지 않는다.<br>
  > 서브클래스 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 이 때문이다.<br><br>

5. 서브클래스의 인스턴스 초기화<br>
  > super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다. this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가한다.<br><br>

6. 인스턴스 반환<br>
  > 완성된 인스턴스가 바인딩된 `this가 반환`된다.<br><br>
