
# 이터레이션 프로토콜

ES6에서 도입된 `이터레이션 프로토콜`은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript사양에 정의하여 미리 약속한 규칙<br><br>

ES6 이전 순회가능한 데이터 컬렉션은 배열, 문자열, 유사배열, DOM 컬렉션 은 `각자의 규칙대로 구조`를 가져 `복잡`했음<br><br>

ES6 이후 `이터레이션 프로토콜`을 준수하는 이터러블로 `통일`하여 <br>
`for...in`, `스프레드`, `배열 디스트럭처링 할당`의 대상으로 사용할 수 있도록 `일원화` 했다.<br><br>

- 이터러블 프로토콜<br>
  Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현 또는,<br>
  프로토타입 체인을 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 `이터레이터`를 반환<br>
  이터러블 프로토콜을 준수한 객체를 `이터러블`이라하며,<br>
  for...in문 스프레드, 디스트럭처링 할당의 대상으로 사용 가능<br><br>

- 이터레이션 프로토콜<br>
  이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.<br>
  이터레이터는 next 메서드를 이용해 순회하며 `value`, `done` 프로퍼티를 갖는 `이터레이터 리절트 객체` 반환<br>
  이터레이터 프로토콜을 준수한 객체를 이터레이터라고 부른다.<br>
  ***이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할***을 한다.<br><br>

<br>

## 이터러블

30장 마지막 예제에서 했던 것처럼 Symbol.iterator 를 프로퍼티 키로 사용한 메서드를 직접 구현<br>
또는, 프로토타입 체인을 통해 상속받은 객체로 이터러블 프로토콜을 준수한다.<br><br>

이터블인지 확인하는 함수<br>
```JavaScript
// 전달받은 인수 v가 null이 아니거나 Sysbol.iterator를 가지고 있을 때 이터러블
const isIterable = (v) => v !== null && typeof v[Symbol.iterator] === "function";

console.log(isIterable([])); // true
console.log(isIterable("")); // true
console.log(isIterable(new Map())); // true
console.log(isIterable(new Set())); // true
console.log(isIterable({})); // false (일반 객체는 기본적으로 이터러블 X)
```

<br>

- 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블<br>
- for...of문으로 순회가 가능<br>
- 즉, 배열, 문자열, Map과 Set 또한 `이터러블`이다.<br><br>

- 일반 객체는 Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않기 때문에 `이터러블이 아니다`.<br>
- 따라서, 일반객체는 순회가 불가능하다.<br><br>

2021년 기준으로 `일반 객체`에도 `스프레드 문법`의 사용이 `가능`해졌다.<br>

```JavaScript
const obj = { a: 1, b: 2 };

// 객체 디스트럭처링
console.log({ ...obj }); // { a: 1, b: 2 }
```

<br>

## 이터레이터

1. 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 `반환`한다.<br>
2. 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 `next 메서드`를 갖는다.<br><br>

```JavaScript
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
console.log('next' in iterator); // true
```

이터레이터의 next 메서드는 각 요소를 순회하기 위한 **포인터** 역할을 한다.<br>
 - next메서드는 순회하며, `이터레이터 리절트 객체`를 반환한다.<br><br>

```JavaScript
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환하고 next 메서드를 가진다.
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next()); // { value : 1, done : false}
console.log(iterator.next()); // { value : 2, done : false}
console.log(iterator.next()); // { value : 3, done : false}
console.log(iterator.next()); // { value : undefined, done : false}
```

<br>

## 빌트인 이터러블

JS는 이터레이션 프로토콜을 `준수`한 객체인 빌트인 `이터러블`을 제공한다.<br><br>

- Array       - Array.prototype[Symbol.iterator]<br>
- String      - String.prototype[Symbol.iterator]<br>
- Map         - Map.prototype[Symbol.iterator]<br>
- Set         - Set.prototype[Symbol.iterator]<br>
- TypedArray  - TypedArray.prototype[Symbol.iterator]<br>
- arguments   - arguments.prototype[Symbol.iterator]<br>
- DOM 컬렉션  - NodeList.prototype[Symbol.iterator]<br>
- DOM 컬렉션  - HTMLCollection.prototype[Symbol.iterator]<br><br>

<br>

## for...of 문

for...of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.<br><br>

```JavaScript
for (const iter of [1, 2, 3]) {
  console.log(iter); // 1 2 3
}
```

for...of 문은 내부적으로 이터레이터의 `next 메서드`를 `호출`하여 이터러블을 순회<br>
next 메서드가 반환한 이터레이터 리절트 객체의 value 값을 변수에 할당하여 사용<br><br>

> 이터레이터 리절트 객체의 `done 값`이 `true`이면 이터러블 순회를 `중단`한다.

<br>

## 이터러블과 유사 배열 객체

유사 배열 객체는 이터러블이 아닌 일반 객체다.<br>
따라서, 유사 배열 객체에는 Symbol.iterator 메서드가 없기 <br>
때문에 for...of 문으로 순회할 수 없다.<br><br>

단, `arguments`, `HTMLCollection`, `NodeList` 객체는 유사배열 객체 이면서 이터러블이다.<br><br>

유사배열 객체를 iterator를 가진 순회가능한 배열로 만드려면<br>
Array.from 메서드가 유용하다.<br><br>

```JavaScript
// 유사 배열 객체
const arrayLike = {
  0： 1,
  1： 2,
  2： 3,
  length: 3
}；

// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

## 이터레이션 프로토콜의 필요성

ES6 이전에는 `각자 나름의 구조`를 가지고 for문, for..in, forEach 메서드로 순회가 가능했지만<br><br>

ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 `이터러블로 통일`하여<br>
for... of 문, 스프레드 문법. 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.<br><br>

즉, `이터레이션`은 `데이터 소비자[for..of, etc]`와 `데이터 공급자[Array, String]`를 연결하는 `인터페이스의 역할`을 한다.<br><br>

- ES6 이후 이터레이션 프로토콜을 준수하는 이터러블로 통일하면서 <br>
- `데이터 공급자`가 `이터레이션 프로토콜`을 `준수`하도록 하면 <br>
- `데이터 소비자`는 `이터레이션 프로토콜`만 `지원`하도록 구현하면 된다.<br>

다시말해<br>

[데이터 소비자]<br>
for...of<br>
spread연산자<br>
디스트럭처링 할당<br>
Map/Set 생성자<br><br>

[인터페이스]<br>
이터레이션 프로토콜<br><br>

[데이터 공급자]<br>
Array, String, Map/Set, Dom Data<br>

`순으로` `연결`만 하면 된다.<br><br>

## 사용자 정의 이터러블

일반 객체도 이터레이션 프로토콜을 준수하도록만 하면 이터러블이 된다.<br><br>

`Symbol.iterator` 메서드를 `구현`하고, Symbol.iterator 메서드가 <br>
`next 메서드`를 갖는 `이터레이터`를 `반환`하도록 구현하면 된다.<br><br>

```JavaScript
// 피보나치 수열을 구현한 사용자 정의 이터러블 [책 page:623]
// 사용자 정의 이터러블 구현
const fibo = {
  // Symbol.iterator 메서드를 직접 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1]; // 배열 디스트럭처링 할당
    const max = 10; // 수열의 최대값

    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고,
    // next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        // 이터레이터 리절트 객체를 반환
        return { value: cur, done: cur >= max };
      },
    };
  },
};

// 이터러블인 fibo 객체를 순회할 때마다 next 메서드가 호출된다.
for (const num of fibo) {
  console.log(num); // 1 2 3 5 8
}
```

