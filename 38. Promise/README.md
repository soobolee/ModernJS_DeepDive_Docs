

# 프로미스

<br>

JS는 비동기 처리를 위한 하나의 패턴으로 `콜백함수`를 사용한다.<br>
하지만 콜백 헬 등 가독성이 나쁘고, 에러의 처리가 `곤란`하고,<br>
여러 개의 비동기처리를 한 번에 하는 것에 `한계`가 있다.<br><br>

따라서 ES6 에서는 비동기 처리를 위한 또 다른 패턴으로 `프로미스[Promise]`를 도입하였다.<br><br>

- 프로미스는 콜백패턴이 가지던 가독성 문제와 에러 처리가 불편하다는 단점을 `보완`하였다.<br>
- 또한 비동기 처리를 한다는 표현을 명확히 표현할 수 있는 `장점`이 있다.<br><br>


## 비동기 처리를 위한 콜백 패턴의 단점

<br>

### 콜백 헬

기본적으로 비동기 함수는 내부의 비동기로 동작하는 코드가 완료되지 않아도, `즉시 종료`된다.<br>
즉, 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.<br><br>

따라서, 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위스코프의 변수에 할당하면<br>
기대한 대로 동작하지 않는다. `like 아래 예제`<br><br>

```JavaScript
let g = 0;

setTimeout(() => {
  g = 100;
}, 0);

console.log(g); // 0
```

<br>

때문에, 위 현상을 해결하기 위해 비동기로 동작하는 함수 내부에 아래 예제처럼 `콜백함수를 삽입`해야 한다.<br><br>

```JavaScript
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open();
  xhr.send("GET", url);

  xhr.onload = () => {
    if (xhr.status === 200) {
      successCallback(JSON.parse(xhr.response));
    } else {
      failureCallback(xhr.status);
    }
  };
};


get("https://jsonplaceholder.typicode.com/posts/1", console.log, console.error);
// id 가 1인 post를 취득
// 서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get에 전달한다.

// {
//   userId: 1,
//   id: 1,
//   title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
//   body: 'quia et suscipit\nsuscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto'
// };
```

<br>

위에 예제는 비동기 처리에 대해 좋은 핸들링을 경험할 수 있기도 하지만 콜백 처리해야 할 코드의 양이 많다면 `가독성을 하락`시킬 수 있다.<br>
또한, 순차적인 실행이 보장되어야 하는 여러개의 함수가 있을 경우 `콜백 헬`을 만들 수 있다.<br><br>

### 에러 처리의 한계

비동기 처리를 위한 콜백 패턴의 문제점 중 가장 심각한 것은 `에러처리가 곤란`하다는 것이다.<br><br>

```JavaScript
try {
  setTimeout(() => {throw new Error("Error!");}, 1000);
} catch (error) {
  console.error("캐치 에러 :", error);
}
```

<br>

위 예제는 catch에 의해 에러가 `캐치되지 않는다.` 그 이유는 setTimeout은 `비동기 함수이기 때문`이다.<br><br>

에러는 `호출자 방향으로 전파`되는데, 비동기 함수인 setTimeout은 콜백 함수가 호출되는 것을 기다리지 않고, 종료되어 콜 스택에서 `제거`된다.<br>
따라서 에러가 타고 가야할 방향이 사라진 것이다. 때문에 콜백함수에서 발생한 에러는 catch 블록으로 `전파되지 않는다.`<br><br>


## 프로미스 생성

Promise 생성자 함수를 new 키워드와 함께 호출하여 프로미스 객체를 생성한다.<br>
프로미스는 호스트 객체가 아닌 ECMAScript 사양에 정의된 `표준 빌트인 객체`이다.<br><br>

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데, 이 콜백 함수는 `resolve`, `reject`를 인수로 전달받는다.<br><br>
```JavaScript
const promise = new Promise((resovle, reject) => {
  if("비동기 처리 성공이다") {
    resolve('result');
  } else {
    reject('failure Error');
  }
})
```

<br>

비동기 처리가 `성공`하면 `resolve` `실패`하면 `reject`를 호출한다. <br><br>

> 프로미스는 다음과 같이 현재 비동기 처리의 진행을 나타내는 상태정보를 가진다.
>
> - pending
>   비동기 처리가 아직 수행되지 않은 상태
>  
> - fulfilled
>   비동기 처리가 수행되어 성공한 상태
>  
> - rejected
>   비동기 처리가 수행되어 실패한 상태

<br>

기본적으로 프로미스는 `pending 상태`이며 비동기 처리가 수행되면 결과에 따라 상태가 변경된다.<br><br>

- `처리 -> 성공 resolve -> fulfilled 상태`<br>
- `처리 -> 실패 reject  -> rejected 상태`<br><br>

> 성공 or 실패 결과에 상관 없이 이미 처리된 상태를 `settled 상태라`고 부른다.
> 한 번 settled 상태가 되면 다시 pending 상태로 돌아가는 것은 `불가능`하다.

<br>

```JavaScript
const fulfilled = new Promise((resolve) => resolve(1));
console.log(fulfilled);

// __proto__: Promise
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: 1


const rejected = new Promise((_, reject) => reject(new Error("failure Error")));
console.log(rejected);

// __proto__: Promise
// [[PromiseState]]: "rejected"
// [[PromiseResult]]: Error: failure Error
```

<br>

## 프로미스 후속 처리 메서드

프로미스의 `비동기 처리 상태가 변경되면` 즉, fulfilled 또는 rejected 상태가 되면, 그에 따른 `후속 처리`를 해야한다.<br><br>

`성공`이면 성공에 따른 로직으로<br>
`실패`면 에러처리에 따른 로직으로<br><br>

움직여야 한다.<br><br>

이를 위해서 promise객체는 `then`, `catch`, `finally` 메서드들을 `제공`해준다.<br><br>

### Promise.prototype.then

then 메서드는 두 개의 콜백함수를 인수로 전달받는다.<br><br>

- `첫 번째 콜백함수는 프로미스가 fulfilled 상태`가 되면 호출된다. 이 때 콜백함수는 프로미스 비동기처리 함수의 `결과`를 인수로 받는다.<br>
- `두 번째 콜백함수는 프로미스가 rejected 상태`가 되면 호출된다. 이 때 콜백함수는 프로미스 비동기처리 함수의 `에러`를 인수로 받는다.<br><br>

```JavaScript
// fulfilled
new Promise((resolve) => resolve("fulfilled"))
  .then(
    (v) => console.log(v),
    (e) => console.error(e),
  );

// Error: rejected
new Promise((_, reject) => reject(new Error("rejected")))
  .then(
    (v) => console.log(v),
    (e) => console.error(e),
  );
```

<br>

then 메서드는 `언제나 프로미스를 반환`한다.<br><br>

then 의 콜백함수가 프로미스를 반환하면 그대로 전달하고<br>
다른 객체나 값을 반환하면 `암묵적`으로 resovle, reject하여 `프로미스를 생성해 반환`<br><br>

### Promise.prototype.catch

catch 메서드는 한 개의 콜백 함수를 인수로 잔달받고, Promise의 상태가 `rejected인 경우에만 호출`된다.<br>
then과 마찬가지로 `언제나 프로미스`를 반환한다.<br><br>

```JavaScript
// 생김새를 보면 위에서 배운 then으로 대체하여 사용할 수 있다.
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) => console.log(e)); // Error: rejected
```

### Promise.prototype.finally

finally 메서드는 한 개의 콜백 함수를 인수로 전달받고, 프로미스의 상태와 `상관없이 무조건 호출`된다.<br>
마찬가지로 `프로미스를 반환`한다.<br><br>

```JavaScript
// 생김새를 보면 위에서 배운 then으로 대체하여 사용할 수 있다.
new Promise(() => {}).finally(() => console.log('finally')); // finally
```

<br>

## 프로미스 에러 처리

```JavaScript
promiseGet('비동기 처리~~')
  .then(res => console.xxx("err"))
  .catch(console.error(err));

// console.xxx Error
```

<br>

then의 두 번째 인수로도 에러를 처리할 수 있지만 then메서드보다는 `catch 메서드를 권장`한다.<br><br>

그 이유로는, 높은 가독성과 명확히 의도를 드러낼 수 있고, 또한 `then의 에러도 잡을 수 있기 때문`이다.<br><br>

## 프로미스 체이닝

프로미스 체이닝 : `then`, `catch`, `finally` 후속 처리 메서드는 언제나 프로미스를 반환하기 때문에<br>
연속적으로 호출이 가능하다 `연속 호출`을 체이닝이라고 한다.<br><br>

- 프로미스는 체이닝을 통해 비동기 처리를 하기 때문에 콜백 패턴의 문제였던 `콜백 헬이 발생하지 않는다.`<br><br>

```JavaScript
// id가 1 인 post의 userid를 취득
promiseGet( ${url}/posts/l')
.then(({ userid }) => promiseGet( ${url}/users/${userld} ))
.then(userlnfo => console.log(userInfo))
.catch(err => console.error(err));
```

<br>

- 하지만 프로미스도 콜백 패턴을 사용하긴 한다.<br>
이 문제는 `ES8`에서 도입된 `async/await` 를 통해 해결이 가능하다.<br><br>

```JavaScript
const url = 'https://jsonplaceholder.typicode.com’;

(async () => {
  // id가 1인 post의 userld를 취득
  const { userid } = await promiseGet( ${url}/posts/l');

  // 취득한 post의 userid로 user 정보를 취득
  const userinfo = await promiseGet( ${url}/users/${userld} );

  console.log(userInfo);
})()；
```

<br>

## 프로미스 정적 메서드

- `Promise.resolve / Promise.reject`<br><br>

Promise.resolve와 Promise.reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.<br>
Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성한다.<br>

- Promise.all<br><br>

Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬처리할 때 사용한다<br><br>

- `Promise.race`<br><br>

Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. <br>
Promise.race 메서드는 Promise.all 메서드처럼 모든 프로미스가 fulfilled 상태가 되는것을 <br>
기다리는 것이 아니라 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다.<br><br>

- `Promise.aUSettled`<br><br>

Promise.aUSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.<br>
그리고 전달받은 프로미스가 모두 `settled 상태(비동기 처리가 수행된 상태, 즉 fulfilled 또는 rejected 상태)`<br>
가 되면 처리 결과를 `배열`로 반환한다.<br><br>


## 마이크로태스크 큐

```JavaScript
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));

// 2 3 1
```

<br>

위 예제를 보면 `1, 2, 3`이 출력될것 같지만 `2, 3, 1이 출력`된다.<br><br>

그 이유는 `프로미스 후속처리 메서드의 콜백 함수는` 태스크 큐가 아닌 `마이크로태스크 큐`에 저장된다.<br><br>

마이크로태스크 큐는 태스크 큐와는 `별개로 브라우저에서 제공`하는 큐이다.<br><br>

각 큐의 `우선순위`는 `마이크로태스크 큐 > 태스크 큐`이기 때문에 <br>
이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.<br>
이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.<br><br>

## fecth

`fetch` 함수는 XMLHttpRequest 객체와 마찬가지로 `HTTP 요청 전송을 제공`하는 `클라이언트 사이드 Web API`이다.<br>
`fetch` 함수는 사용법이 간단하고, `프로미스를 지원`하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.<br><br>

```JavaScript
const promise = fetch(url [, options]);

// fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

fetch("https://jsonplaceholder.typicode.com/todos/1")
  .then((response) => console.log(response));
```

<br>

response 객체의 Response.prototype 에는 Response 객체에 포함되어 있는 HTTP 응답 몸체를 위한 다양한 메서드를 제공한다.<br>
fetch 함수가 반환한 프로미스가 래핑하고 있는 MIME 타입이 application/json인 HTTP 응답 몸체를 획득하려면<br>
`Response.prototype.json` 을 사용한다. 이 메서드는 Response객체에서 `응답 Body`를 획득하여 역직렬화 한다.<br>

