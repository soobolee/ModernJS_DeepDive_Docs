
# 디스트럭처링 할당

`디스트럭처링 할당`은 구조화된 배열과 같은 이터러블 또는 객체를 <br>
destructuring(`비구조화`, `구조 파괴`) 하여, 1개 이상의 변수에 `개별적`으로 `할당`하는 것을 말한다.<br><br>

배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당 시 유용하다.<br><br>

## 배열 디스터럭처링 할당

- 할당의 대상은 항상 `우변`이어야 함.<br>
- 좌변은 `배열 리터럴` 형태여야 함.<br>
- 배열 디스트럭처링 할당의 대상은 이터러블이어야 하며, 할당 기준은 배열의 `인덱스`로 `순서대로` 할당<br><br>

```JavaScript
ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var thr = arr[2];

ES6 array destructuring
const arr = [1, 2, 3];

const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

배열의 개수와 할당 변수의 개수가 `일치하지 않아도 된다.`<br>
알아서 순서대로 들어간다.<br>

```JavaScript
const arr = [1, 2, , 3];

const [one, two, three] = arr;

console.log(one, two, three); // 1 2 undefined
```

변수에 `기본값`을 설정 가능하다.<br>

```JavaScript
const arr = [1, 2, 4];

const [one, two, three = 3] = arr;

console.log(one, two, three); // 1 2 4
```

## 객체 디스트럭처링 할당

객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체에서 추출하여 1개 이상의 변수에 할당한다.<br><br>

배열과 마찬가지로<br>
- 디스트럭처링 대상은 `우변`에 위치<br>
- 변수는 `좌측`에 위치<br>
- 변수는 `객체 리터럴`로 선언해야함<br>
- 배열과 달리 `순서에 상관이 없다`. `프로퍼티 키`가 있기 때문<br><br>

```JavaScript
const user = {
  firstName: "LEE",
  lastName: "SOOBO",
};

const { lastName, firstName } = user;
console.log(lastName, firstName); // SOOBO LEE
```

배열과 마찬가지로 `기본값을` 설정 할 수 있다.<br><br>

```JavaScript
const user = {
  firstName: "LEE",
};

const { lastName = "SOOBO", firstName } = user;
console.log(lastName, firstName); // SOOBO LEE
```

객체 디스트럭처링 할당을 위해 변수에 `Rest파라미터`를 사용가능하다.<br><br>

```JavaScript
// 객체 디스트럭처링 Rest파라미터 적용
const { x, ...rest } = { x: 1, y: 2, z: 3 };

console.log(x, rest); // 1 { y: 2, z: 3 }
```
