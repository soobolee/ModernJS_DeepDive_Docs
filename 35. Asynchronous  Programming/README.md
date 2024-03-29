
# 비동기 프로그래밍

## 동기 처리와 비동기 처리

1. 동기 처리방식<br><br>

- 일반적인 함수의 실행 과정<br>
1. 함수를 호출하면 함수 코드가 평가되어 `함수 실행 컨텍스트`가 `생성`되고,<br>
2. 생성된 함수 실행 컨텍스트는 실행 컨텍스트 스택에 푸시되고, 함수 실행<br>
3. 함수의 실행이 종료되면 함수 실행 컨텍스트는 실행 컨텍스트 스택에서 `팝`되어 제거<br><br>

```JavaScript
const foo = () => {};
const bar = () => {};

foo();
bar();
```

<br>

<img width="658" alt="스크린샷 2024-03-27 오후 9 29 57" src="https://github.com/soobolee/ModernJS_DeepDive_Docs/assets/86949902/c0b6f45b-b733-497d-a8ef-68b1b4152f23">

사진으로 보다 싶이 자바스크립트 엔진은 `단 하나의 실행 컨텍스트 스택`을 가진다.<br><br>

즉,<br>
- `동시에 2개 이상의 함수 실행 불가`를 의미<br>
- 실행 컨텍스트 최상위의 실행 중인 컨텍스트를 제외한 나머지는 `대기 중`<br>
- 위에 함수가 종료되어 pop되어야 `비로소 아래 함수가 시작`<br><br>

이러한 JS의 동작 방식을 `싱글 스레드`라고 한다.<br><br>

이처럼 싱글 스레드이기 때문에 실행 중인 테스크가 종료할 때까지 다음 실행될 테스크가 대기하는 방식을<br>
`동기처리 방식`이라고 부른다.<br><br>



```JavaScript
function sleep (func, delay) {
  const delayUntil = Date.now() + delay;

  while (Date.now() < delayUntil);

  func();
}

function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

sleep(foo, 3 * 1000);
bar();
```

<br>

위 예제에서 foo 함수는 3초 후에 실행된다. <br>
이때 bar 함수는 sleep함수의 실행이 종료된 이후 3초 이후에 호출된다.<br><br>

<img width="633" alt="스크린샷 2024-03-27 오후 9 47 39" src="https://github.com/soobolee/ModernJS_DeepDive_Docs/assets/86949902/4d3a1e11-bc69-415d-a9c9-87bb98545f64">


동기처리 장점<br>
- 테스크들의 실행순서가 보장된다.<br><br>


2. 비동기 처리<br>

```JavaScript
function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

setTimeout(foo, 3 * 1000);
bar();
```

setTimeout 함수는 이후의 테스크를 블로킹하지 않고 곧바로 실행한다.<br>
이처럼 테스크가 종료되지 않은 상태라 해도 다음 테스크를 곧바로 실행하는 방식을<br>
`비동기처리 방식`이라고 한다.<br><br>

<img width="568" alt="스크린샷 2024-03-27 오후 9 51 17" src="https://github.com/soobolee/ModernJS_DeepDive_Docs/assets/86949902/1e51fd36-f8f9-4936-bf37-f6489cd4da4a">

비동기처리 장점<br>
- 블로킹이 없기 때문에 속도가 빠르다.<br><br>


## 자바스크립트 엔진 구성

1. `콜 스텍`<br>
  - `실행 컨텍스트`가 추가되고 제거되는 스택 자료구조이다.<br>
  - 함수를 호출하면 순차적으로 함수 실행 컨텍스트가 콜 스택에 쌓인다.<br>
  - JS엔진은 단 하나의 콜 스택만 있기 때문에 최상위 실행 컨텍스트를 우선 처리한다.<br><br>

2. `힙 메모리`<br>
  - `객체`가 저장되는 `메모리 공간`이다. 콜 스택의 컨텍스트들은 힙 메모리를 참조한다.<br>
  - 메모리가 값을 저장 시 메모리의 크기를 결정해야한다.<br>
  - 객체는 원시값과 다르게 크기가 정해져 있지 않고, 동적할당 된다.<br>
  - 즉, 객체가 저장되는 메모리 공간인 힙은 구조화 되어 있지 않다.<br><br>

3. `테스크 큐`<br>
  - `비동기 함수의 콜백 함수` 또는 `이벤트 핸들러`가 일시적 `보관되는 장소`<br>
  - 별도로 프로미스의 후속 처리 메서드의 콜백 함수가 일시적으로 보관되는 마이크로테스크 큐도 존재<br><br>

4. `이벤트 루프`<br>
  - `콜 스택`이 비어있는지 계속 `확인`하고 비었다면 `테스크 큐`에 대기 중인 함수를 `이동`시킨다.<br>
  - 테스크 큐에서 `FIFO`방식으로 콜 스택으로 이동<br><br>

<img width="551" alt="스크린샷 2024-03-27 오후 9 55 43" src="https://github.com/soobolee/ModernJS_DeepDive_Docs/assets/86949902/a1c000d5-c022-4a5a-b84c-bbf1973ae116">

<br>

```JavaScript
function foo() {
  console.log('foo');
}
function bar() {
  console.log('bar');
}
setTimeout(foo, 0); // 0초 실제는 4ms 후에 foo 함수가 호출된다.
bar();

// 'bar'
// 'foo'
```

- 실행 순서<br>
1. 전역 코드가 평가되어 전형 실행 컨텍스트가 생성되고 콜 스택에 푸시된다.<br><br>

2. 전역 코드가 실행되기 시작하여 setTimeout 함수가 호출된다. setTimeout 함수의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 중인 실행 컨텍스트가 된다. 브라우저의 Web API인 타이머 함수도 실행 컨텍스트를 생성한다.<br><br>

3. SetTimeout 함수가 실행되면 콜백 함수는 호출 스케줄링하고 종료되어 콜 스택에서 팝된다. 타이머 설정과 타이머가 만료되면 콜백 함수를 태스크 큐에 푸시하는 것은 브라우저의 역할이다.<br><br>

4. 브라우저가 수행하는 4-1과 자바스크립트 엔진이 수행하는 4-2는 병행 처리된다.<br>
  4-1 브라우저는 타이머를 설정하고 타이머의 만료를 기다리며 타이머가 만료되면 foo가 태스크 큐에 푸시된다. 위 경우 최소 지연 시간 4ms가 지정된다. 따라서 4ms 동안 콜백 함수 foo가 태스크 큐에 푸시되어 대기하게 된다. 이처럼 setTimeout 함수로 호출 스케줄링한 콜백 함수는 정확히 지연 시간 후에 호출된다는 보장은 없다.<br>
  4-2 bar 함수가 호출되어 bar 함수의 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 컨텍스트가 된다. 이후 bar 함수가 종료되어 콜 스택에서 팝 된다.<br><br>

5. 전역 코드 실행이 종료되고 전역 실행 컨텍스트가 콜 스택에서 팝 되어 아무 실행 컨텍스트도 존재하지 않게 된다.<br><br>

6. 이벤트 루프에 의해 콜 스택이 비어 있음이 감지되고 태스크 큐에서 대기 중인 콜백 함수 foo가 이벤트 루프에 의해 콜 스택에 푸시 된다.<br><br>

7. setTimeout의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게 되면( 전역 코드 및 명시적 호출 함수가 모두 종료하면) 콜 스택에 푸시되어 실행된다.<br><br>

8. 자바 스크립트는 싱글 스레드 방식으로 동작하지만 브라우저는 멀티 스레드 방식으로 동작한다.<br>

<hr>

이처럼 비동기 함수인 setTimeout의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게 되면,<br>
다시 말해 전역 코드 및 명시적으로 호출된 함수가 모두 종료하면 비로소 콜 스택에 푸시되어 실행된다.<br>
<br>
자바스크립트는 `싱글 스레드 방식`으로 동작한다.<br>
이때 싱글 스레드 방식으로 동작하는 것은 브라우저가 아니라 <br>
브라우저에 내장된 `자바스크립트 엔진`이라는 것에 주의하기 바란다.<br><br>

만약 모든 자바스크립트 코드가 자바스크립트 엔진에서 싱글 스레드 방식으로 동작한다면 자바스크립트는 비동기로 동작할 수 없다.<br>
즉, 자바스크립트 엔진은 `싱글 스레드`로 동작하지만 브라우저는 `멀티 스레드`로 동작한다.<br><br>
