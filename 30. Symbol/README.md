

# Symbol

ES6에 추가된 `7번째 데이터 타입`이다.<br><br>

변경 불가능한 원시 타입의 값으로 다른 값과 중복되지 않는 `유일무이한 값`이다.<br>
주로, 이름중복 충돌이 없어야 하는 프로퍼티 키를 만들기 위해 주로 사용한다.<br><br>

## Symbol 값의 생성

Symbol값은 다른 원시타입들과 다르게 `리터럴 표기법을 지원하지 않는다.`<br>
따라서, 오로지 `Symbol 함수`로만 생성한다.<br><br>

이때 생성된 Symbol값은 외부로 노출되지 않아, 유일무이한 값이 된다.<br><br>

```JavaScript
// 심벌 값 생성 - Symbol 함수
const symbol = Symbol();
console.log(typeof symbol); // symbol

// 심벌 값은 외부로 노출 X
console.log(symbol); // Symbol()

// new와 함께 생성자 함수로 생성 불가
console.log(new Symbol()); // TypeError: Symbol is not a constructor

// 원시타입의 값이지만 다른 원시타입처럼 객체처럼 접근하면 래퍼 객체를 생성한 후 돌아온다.
const mySymbol = Symbol("mySymbol");

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)

// 암묵적으로 boolean을 제외한 다른 타입으로 변환되지 않아서 안전하다.
const mySymbol = Symbol();

console.log(!!mySymbol); // true
console.log(mySymbol + ""); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a string
```

## Symbol.for / Symbol.keyFor 메서드

- Symbol.for<br>
  인수로 전달받은 문자열을 키로 사용하여 해당 키와 일치하는 심벌 값을 검색한다.<br>
  - 성공 시 : `검색된 Symbol`값을 봔한<br>
  - 실패 시 : 새로운 Symbol값을 생성하고 전달받은 인수를 key로 하는 `새로운 Symbol을 반환`<br>
  ```JavaScript
  // 전역 심벌 레지스트리에 symbol이 없으니 새로운 심벌 값 생성 후 반환
  const symbol1 = Symbol.for("symbol");
  // 위 코드에서 symbol이 생성 되었으니 같은 symbol 반환
  const symbol2 = Symbol.for("symbol");

  // 결론적으로 같다.
  console.log(symbol1 === symbol2); // true
  ```

- Symbol.keyFor<br>
  Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.<br>
  ```JavaScript
  const s1 = Symbol.for('symbol');

  // 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
  Symbol.keyFor(s1); // symbol

  const s2 = Symbol('foo');

  Symbol.keyFor(s2); // Symbol함수를 생성한 symbol은 전역 심벌 레지스트리에서 등록, 관리되지 않음
  ```


## Symbol과 상수

```JavaScript
// 상수 정의
// 1,2,3,4 라는 상수는 특별한 의미도 없고, 상수 이름에 의미가 있다. 때문에 값이 중복될 수 있다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};
```
값에는 특별한 의미가 없고, 상수 이름 자차에 의미가 있는 경우, <br>
값이 중복되거나 변경될 수 있으므로, <br>
상수 대신 중복될 위험이 없는 Symbol을 사용한다.<br><br>

## Symbol과 프로퍼티 키

```JavaScript
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // 1
obj.mySymbol; // undefined
```

Symbol값을 동적으로 생성하여 프로퍼티 키를 생성할 수도 있다.<br><br>

유일무이하고 중복 충돌 위험 없는 프로퍼티 키를 생성할 수 있다.<br><br>

## Symbol과 프로퍼티 은닉

Symbol 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 외부로 노출되지 않는다.<br>
따라서, for...in, Object.keysm Object.getOwnPropertyNames 메서드로 찾을 수 없다.<br><br>

때문에 우리는 Symbol을 사용해 프로퍼티 키를 은닉했다고 표현한다.<br><br>

하지만 완전히 숨기는 것은 아니다.<br>
Object.getOwnPropertySymbols 메서드를 이용하면, 심벌값을 프로퍼티 키로 사용한 프로퍼티 값을 찾을 수 있다.<br>

```JavaScript
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 못찾음
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []

// ES6 에서 추가된 메서드임
console.log(Object.getOwnPropertySymbols(obj)); // [ Symbol(mySymbol) ]
```

## Symbol과 빌트인 객체 확장

일반적으로 중복 위험이 있기 때문에 표준 빌트인 객체에 사용자 정의 메서드를 추가하는 것은 권장하지 않는다.<br>
기존 메서드가 덮어 씌워질 가능성이 있기 때문이다.<br><br>

하지만 Symbol을 사용하여 유일무이한 사용자 정의 메서드를 만들어 빌트인 객체 확장을 할 수 있다.<br><br>

```JavaScript
// Array 표준 빌트인 객체에 메서드를 추가 : 권장 X
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
}[1, 2].sum(); // 3

// 심벌 값으로 프로퍼티 키를 갖는 메서드로 확장
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
}
[1, 2][Symbol.for("sum")](); // 3
```

## Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 Symbol 값을 ECMAScript 사양에서는 `Well-known Symbol` 이라고 한다.<br>
Well-known Symbol은 내부 자바스크립트 엔진에 사용된다.<br><br>

for...of 문으로 순회 가능한 `이터러블`은 Well-known Symbol 인 `Symbol.iterator`를 키로 갖는 메서드를 가진다.<br><br>

Symbol.iterator 메서들 `호출`하면 `이터레이터`를 `반환`하도록 사양에 규정되어 있다.<br>
빌트인 이터러블은 즉, 이 규정 이터레이션 프로토콜을 준수한다.<br><br>

따라서 만약 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면, 이터레이션 프로토콜을 따르면 된다.<br>
즉 **Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현**하면<br>
그 객체는 `이터러블`이 된다.<br><br>

```JavaScript
// 1 ~ 5 범위의 정수
const iterable = {
  // Symbol.itertor 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;

    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```







