

# 스프레드 문법

앞 챕터에서 봤듯이 스프레드 문법을 사용할 수 있는 대상은<br>
`Array`, `String`, `Map/Set`, `DOM 컬렉션` 같이 `for...of 문`으로 순회가능한 이터러블 객체들이다.<br><br>

ES6에 도입된 스프레드 문법은 하나로 뭉쳐있는 여러 값들의 집합을 전개해서 `개별적인 값을의 목록`으로 만들어 `반환`한다.<br><br>

즉, 스프레드 문법의 결과는 값이 아니다. 따라서, 이 문법의 결과는 `변수에 저장할 수 없다.`<br><br>

```JavaScript
const arr = ...[1, 2, 3]; // SyntaxError
```

위 예제처럼 스프레드 문법의 결과는 값으로 사용이 불가능하고, <br><br>

다음과 같이 쉼표로 구분한 값의 목록에만 사용하는 문맥에서만 사용가능하다.<br><br>

1. 함수 호출문의 인수목록<br>
2. 배열 리터럴의 요소목록<br>
3. 객체 리터럴의 프로퍼티 목록<br><br>

# 1. 함수 호출문의 인수목록

가변인자 함수는 매개변수 개수를 확정하기 힘들다, <br>
`가변인자 함수`로는 Math.max가 있다.<br><br>

```JavaScript
Math.max(1); // 1
Math.max(1, 2); // 2

Math.max([1, 2, 3]); // NaN
```

배열을 인수로 넣으면 최대값을 구할 수 없어 `NaN`을 반환한다.<br>
때문에, 배열을 펼쳐서 넘겨야 하는데 이럴 때 스프레드 문법이 유용하다.<br><br>

```JavaScript
const arr = [1, 2, 3];

console.log(Math.max(...arr)); // 3
```

스프레드 문법 이전에는 Function.prototype.apply 를 사용해서 배열을 분개했다.<br>
```JavaScript
const arr = [1, 2, 3];

// 배열이 펼쳐져서 인수로 전달되는 효과가 있다.
console.log(Math.max.apply(null, arr)); // 3
```

<br>

> `Rest파라미터`와 `혼동하지 말자`
> `Rest파라미터`와 스프레드 문법은 형태가 동일하여 혼동되어 사용될 수 있다.
>
> ```JavaScript
> function foo (...rest) {
>   console.log(rest) // 1, 2, 3 -> [1, 2, 3]
> }
>
> // 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
>  foo(...[1, 2, 3]); // [1, 2, 3] -> 1, 2, 3
> ```

<br>

# 2. 배열 리터럴의 요소목록

스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존 방식보다 더욱 `간결`하고 `가독성` 좋게 표현이 가능하다.<br><br>

- 배열 결합 / concat<br>

```JavaScript
const arr1 = [1, 2];
const arr2 = [3, 4];

// ES5  concat 메서드
let merge = arr1.concat(arr2);
console.log(merge); // [ 1, 2, 3, 4 ]

// ES6  스프레드 문법
merge = [...arr1, ...arr2];
console.log(merge); // [ 1, 2, 3, 4 ]
```

- 배열 요소 추가, 제거 / splice + (apply, spread)<br>

```JavaScript
const arr1 = [1, 4];
const arr2 = [2, 3];

// ES5 splice + apply + concat 메서드
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [ 1, 2, 3, 4 ]

// ES6 splice 메서드 + 스프레드 문법
arr1.splice(1, 0, ...arr2);
console.log(arr1); // [ 1, 2, 3, 4 ]
```

- 배열 복사 / slice<br>

```JavaScript
const arr = [1, 2, 3, 4];

// ES5 slice 메서드
let dup = arr.slice();
console.log(dup); // [ 1, 2, 3, 4 ]

// ES6 스프레드 문법
dup = [...arr];
console.log(dup); // [ 1, 2, 3, 4 ]
console.log(dup === arr); // false
```

- 이터러블 배열 변환 / apply + call + slice<br>

```JavaScript
function sum() {
  // ES5 이터러블 배열로 변환
  const args = Array.prototype.slice.call(arguments);

  return args.reduce((acc, cur) => acc + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

```JavaScript
function sum() {
  // arguments는 이터러블이면서 유사 배열 객체이므로 스프레드 문법 가능
  return [...arguments].reduce((acc, cur) => acc + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

```JavaScript
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// 유사 배열 객체는 이터러블X 때문에 스프레드 X -> 배열로 변환해야 함
const arr = Array.from(arrayLike);
```

# 3. 객체 리터럴의 프로퍼티 목록

스프레드 문법은 원래 일반 객체에서 허용하지 않았지만, <br>
2021년도 부터 `객체의 프로퍼티 목록`에서도 `사용`할 수 있게 되었다.<br><br>

```JavaScript
// 객체 복사 (얕은 복사)
const obj = { x: 1, y: 2 };

const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합 ES5이전에는 Object.assign을 사용하여 병합했음
const merged = { ...obj, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```


