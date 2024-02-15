
# 제어문

`제어문`은 조건에 따라 코드블록을 실행하거나 반복실행 할 때 사용한다.<br />
일반적으로 위 아래로 흐르는 코드실행 `순서를 인의적`으로 제어 할 수 있다.<br />

<hr />

## 블록문

블록문은 0개 이상의 문을 중괄호로 묶은 것으로 코드 블록 또는 블록으로 부른다.<br />
일반적으로 `제어문`이나 `함수`를 정의할 때 사용한다.<br />

```JavaScript
{
  var foo = 10;
}

if (true) {
  
}

function sum (a, b) {
  return a + b;
```

<hr />

## 조건문

주어진 조건식의 `평가결과`에 따라서 코드블록의 실행을 `결정`한다.<br />
if...else , switch 문이 제공된다.<br />

```JavaScript
if (true) {
  // 조거식이 참일 때 실행되는 블록
} else {
  // 조건식이 거짓일 때 실행되는 블록
}
```

삼항연산자로 간단하게 줄여 쓸 수 있다.<br />
보통 삼항연산자는 값으로 평가되기 때문에 변수에 할당하거나 리턴 시 사용한다.<br />

```JavaScript
switch (표현식) {
  case 표현식1：
    switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2：
    switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
    switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```

필자는 주로 조건의 개수가 많을 때 `가독성을 높이기` 위해 `switch`문을 애용한다. if를 중첨해 사용하는 것보다 훨씬 가독성이 좋다
`break`는 필수적이지만 개발자의 `의도`대로 빼서 사용하기도 한다.

<hr />

## 반복문

반복문은 조건식의 평가 결과에 따라 코드 블록을 `반복수행`한다.
for, while, do...while 문을 제공한다.

```JavaScript
for (var i = 0; i < 2; i++){
  console.log(i);
}
```

var i는 iteration의 `i`를 의미한다. i의 중첩으로는 보통 `j`가 일반적이다.<br />
<br />
1. 변수선언문은 맨 처음 단 `한번만` 실행된다.
2. 조건식이 실행되어 평가결과를 반환한다.
3. 실행블록이 실행된다.
4. 증감문이 실행된다.

```JavaScript
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}
```

while문은 반복횟수가 `불명확`할때 주로 사용한다.<br />
조건식과는 별도로 `break;문`을 사용하여 루프를 탎출할 수 도 있다.<br />

<hr />

## continue문

```JavaScript
var string = 'Helo World.';
var search = 'l';
var count = 0;

for (var i = 0; i < string.length; i++) {
  // ’l’이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.

  if (string[i] !== search) contiue;
  count+ + ; // continue 문이 실행되면 이 문은 실행되지 않는다.
}
```

`contiue문`은 반복문의 코드 블록 실행을 현 지점에서 `중단`하고 반복문의 증감식 또는 다음 반복으로 실행 흐름을 `이동`시킨다.<br />
break 문처럼 반복문을 탈출하지는 않는다<br />
