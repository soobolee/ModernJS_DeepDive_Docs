

# 제네레이터란

`제네레이터`는 코드 블록의 실행을 `일시적`으로 `중지`한 후 필요한 시점에 `재개`가능한 `특수함수`이다.<br><br>

## 일반함수 vs 제네레이터함수

1. 제네레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도 가능<br>
  - `일반함수` : 호출하면 제어권은 함수에게 넘어가고 함수 코드 일괄 실행 제어불가X<br>
  - `제네레이터` : 함수 실행을 함수 호출자가 중지, 재개의 제어권을 함수 호출자에게 양도 제어가능O<br><br>

2. 제네레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있습니다.<br>
  - `일반함수` : 매개변수를 통해 외부에서 받은 값을 일괄적으로 함수가 실행하는데 사용<br>
  - `제네레이터` : 함수 호출자와 양방향으로 상태를 주고 받을 수 있다.<br><br>

3. 제네레이터 함수를 호출하면 제네레이터 객체를 반환합니다.<br>
  - `일반함수` : 호출하면 함수 코드를 일괄 실행하고 값을 반환<br>
  - `제네레이터` : 함수를 호출하면 코드를 실행하는 것이 아닌 이터러블이면서, 이터레이터인 제네레이터 객체를 반환<br><br>



## 제네레이터 함수 정의

<br>

```JavaScript
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genDecFunc = function* () {
  yield 1;
}

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
}

// 제너레이터 클래스 메서드
class MyClass {
  * genClassMethod() {
    yield 1;
  }
}

// 애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관 없지만,
// 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장합니다.

// function* genFunc() {
//  yield 1;
// }
```

- 제네레이터 함수는 화살표 함수로 정의가 불가능하다.
 ```JavaScript
const GenArrowFunc = * () => {
 yield 1;
};
// SyntaxError : Unexpected token '*'
```

<br>

- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없습니다.<br>
 ```JavaScript
function* genFunc() {
  yield 1;
}
new genFunc();
// TypeError : genFunc is not a constructor
```

<br>

## 제네레이터 객체
제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 <br>
제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 `제너레이터 객체`는 <br>
`이터러블(iterable)`이면서 동시에 `이터레이터(iterator)`다.<br><br>

- `제네레이터 객체`는 이터레이터와 같이 `next메서드`를 가지지만 이터레이터에 없는 `return`, `throw` 메서드를 `추가`로 갖습니다.<br><br>

- next 메서드<br>
  제네레이터의 `yield[양도]` 표현식까지 코드 블록을 실행하고 `yield된 값`을 `value 프로퍼티 값`으로<br>
  `false를 done 프로퍼티 값`으로 갖는 이터레이터 리절트 객체를 반환합니다.<br><br>
  
- return 메서드<br>
  `인수로 전달받은 값을 value 프로퍼티 값으로 `true를 doen프로퍼티 값`으로 갖는<br>
  이터레이터 리절트 객체를 반환<br><br>

- throw 메서드<br>
  전달받은 에러를 발생시키고, `undefined를 value 프로퍼티 값`,<br>
  `true를 done 프로퍼티 값`으로 갖는 이터리에터 리절트 객체 반환<br><br>

```JavaScript
function* genFunc() {  
  try {  
    yield 1;  
    yield 2;  
    yield 3;  
  } catch (e) {  
    console.error(e);  
  }  
}  
  
const generator = genFunc();  
  
console.log(generator.next()); // {value: 1, done: false}  
console.log(generator.return('End!')); // {value: "End!", done: true}
console.log(generator.throw('Error!')); // {value: undefined, done: true}
```

## 제네레이터의 일시 중지, 재개

제네레이터 함수는 `[양도]yield` 키워드와 `next메서드`를 통해 실행을 일시 `중지`했다가 필요한 시점에 `재개` 가능<br><br>

- [양도]yield 키워드는 제네레이터 함수의 실행을 일시 중지 및 뒤에 오는 표현식의 평가결과를 함수 `호출자에게 반환`한다.<br><br>

```JavaScript
function* genFunc() {  
  yield 1;  
  yield 2;  
  yield 3;  
}  
  
const generator = genFunc();  
  
console.log(generator.next()); // {value: 1, done: false}
// yield 양도
console.log(generator.next()); // {value: 2, done: false}
// yield 양도
console.log(generator.next()); // {value: 3, done: false}
// yield 양도
console.log(generator.next()); // {value: undefined, done: true} -> error
```

<br>

## 제네레이터 함수를 이용한 비동기 제어

제네레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고 받을 수 있다.<br>
프로미스를 사용한 비동기 처리를 동기처럼 구현할 수 있다.<br><br>

```JavaScript
const async = generatorFunc => {
    const generator = generatorFunc();

    const onReserved = arg => {
        const result = generator.next(arg);

        return result.done ? result.value : result.value.then(res => onReserved(res));
    }

    return onReserved;
}

(async (function* fetchTodo() {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = yield fetch(url);
    const todo = yield response.json();
    console.log(todo);
})())
```

1. async 함수가 호출되면 전달받은 제너레이터 함수 fetchTodo를 호출하여 제너레이터 객체를 생성, onReserved 함수를 반환<br>
   onReserved 함수는 상위 스코프의 generator 변수를 기억하는 클로저<br>
   async 함수가 반환한 onReserved 함수를 즉시 호출하여 next 메서드를 처음 호출.<br><br>

2. next 메서드가 처음 호출되면 제너레이터 함수 fetchTodo의 첫 번째 yield 문까지 실행<br>
   이때 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값,<br>
   즉, 첫 번째 yield된 fetch 함수가 반환한 프로미스가 resolve한 Response 객체를 onReserved 함수에 인수로 전달하고 재귀 호출.<br><br>

3. onReserved 함수에 인수로 전달된 Response 객체를 next 메서드에 인수로 전달하고 next 메서드를 두 번째로 호출.<br>
   이때 next 메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo의 response 변수에 할당<br>
   제너레이터 함수 fetchTodo의 두 번째 yield문까지 실행.<br><br>

4. next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값.<br>
   즉, 두 번째 yield된 response.json 메서드가 반환한 프로미스가 resolve한 todo 객체를 onReserved 함수에 인수로 전달하며 재귀 호출.<br><br>

5. onReserved 함수에 인수로 전달된 todo 객체를 next 메서드에 인수로 전달하면서 next 메서드를 세 번째로 호출.<br>
   next 메서드에 인수로 전달한 todo 객체는 제너레이터 함수 fetchTodo의 todo 변수에 할당<br>
   제너레이터 함수 fetchTodo가 끝까지 실행.<br><br>

6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true.<br>
    즉, 제너레이터 함수 fetchTodo가 끝까지 실행되었다면 이터레이터 리절트 객체와 value 프로퍼티 값을 undefined로 반환하고 종료.<br><br>


## async/await

`async/await`를 배우기 위해 위에서 제너레이터 함수를 공부하였다.<br><br>

`ES8`에서 추가된 제네레이터보다 `간단`하고 `가독성` 좋게 비동기 처리를 동기처리처럼 동작하도록 구현하는 키워드<br>
`async/await`를 사용하여 프로미스의 후속 처리 메서드를 사용하지 않고, `동기처럼 사용`가능<br><br>

- `async/await`는 `프로미스를 기반`으로 동작<br><br>

```JavaScript
const fetch = require('node-fetch');  
  
async function fetchTodo() {  
  const url = 'https://jsonplaceholder.typicode.com/todos/1';  
  
  const response = await fetch(url);  
  const todo = await response.json();  
  console.log(todo);  
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}  }  
  
fetchTodo();
```


## async

await 키워드는 반드시 async 함수 `내부에서 사용가능`<br>
async 함수는 함수앞에 async 키워드를 붙여 생성하고 암묵적으로도 `항상 프로미스를 반환`한다.<br><br>

> 클래스의 `생성자[constructor]함수`는 async의 메서드가 될 수 `없다.`<br>
> 생성자 함수는 클래스의 인스턴스를 반환해야 하는데 async 함수가 되어버리면 항상 프로미스를 반환하게 되어버린다.<br><br>

```JavaScript
// async 함수 선언문  
async function foo(n) { return n;}  
foo(1).then(v => console.log(v)); // 1  
  
// async 함수 표현식  
const bar = async function (n) { return n;};  
bar(2).then(v => console.log(v)); // 2  
  
// async 화살표 함수  
const baz = async n => n;  
baz(3).then(v => console.log(v)); // 3  
  
// async 메서드  
const obj = {  
  async foo(n) {return n;}  
};  
obj.foo(4).then(v => console.log(v)); // 4  
  
// async 클래스 메서드  
class MyClass {  
  async bar(n) {return n;}  
}  
  
const myClass = new MyClass();  
myClass.bar(5).then(v => console.log(v)); // 5
```

## await

await 키워드는 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리결과를 반환하고,<br>
`항상 프로미스 앞에 키워드`를 선언 해야한다.<br><br>

```JavaScript
const fetch = require('node-fetch');  
  
const getGithubUserName = async id => {  
  const res = await fetch(`https://api.github.com/users/${id}`);  
  const {name} = await res.json();  
  console.log(name); // SooBo
}  
  
getGithubUserName('soobolee');
```

await 키워드는 프로미스가 settled 상태를 기다린 후 settled상태가 되면<br>
프로미스가 resolve한 결과를 res변수에 할당한다.<br><br>

> 즉, await 키워드는 프로미스의 실행을 기다리며 `함수의 실행을 일시 중지`시킨다.<br><br>

```JavaScript
async function foo() {
  const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
  const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
  const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}
foo(); // 약 6초

// 위 처럼 연관 없는 코드들에는 Promise.all()을 활용해 병렬처리할 수 있다.

async function bar() {
  const res2 = Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 3000));
    new Promise(resolve => setTimeout(() => resolve(2), 2000));
    new Promise(resolve => setTimeout(() => resolve(3), 1000));
  ])

  console.log(res2); // [1, 2, 3]
}

bar(); // 약 3초
```

<br>

## 에러 처리

비동기 처리를 위한 `콜백 패턴`의 `단점`은 `에러 처치가 곤란한 점`이 있었다.<br>
에러는 `호출자 방향`으로 전파되기 때문인데, <br>
콜백함수를 호출한 호출자는 이미 메모리에서 `pop` 되어 있기 때문이다.<br><br>

```JavaScript
try {  
  setTimeout(() => {throw new Error('Error');}, 3000);  
} catch (e) {  
  // 에러캐치 불가
  console.error('catch ERROR', e);  
}
```

따라서, `try...catch문`으로 에러를 잡을 수 없었다.<br><br>

하지만 `async/await`에서 에러 처리는 `try...catch문을 사용할 수 있다.`<br><br>

프로미스를 반환하는 비동기 함수는 명시적으로 호출할 `호출자가 명확`하기 때문이다.<br><br>

```JavaScript
const foo = async () => {  
  try {  
    const wrongUrl = 'https://wrong.url';  
  
    const response = await fetch(wrongUrl);  
    const data = await response.json();  
    console.log(data);  
  } catch (e) {  
    console.error(e); 
  }  
};  
  
foo(); //TypeError: Failed to fetch  
```

위 처럼 에러를 try문 안에 에러를 캐치할 수 있다.<br><br>

만약 catch 문이 존재하지 않는다면 async는 반환 프로미스로 `reject하는 프로미스를 반환`한다.<br><br>

따라서, Promise와 마찬가지로 `then` 또는 `catch` `후속 메서드로 에러처리가 가능`하다.<br>


