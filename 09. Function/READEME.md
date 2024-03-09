
# 함수란

<br>

수학의 함수와 같은 개념으로 `입력`을 받아 `출력`을 내보내는 일련의 과정을 정의한 것이다.<br>
즉, 매개변수를 `인수`로 받아 출력을 `반환값`으로 정의한다.<br>

<br>

## 함수를 사용하는 이유
- `코드의 재사용`<br>
  동일한 작업을 반복적으로 수행하는 코드를 함수로 만들어 호출하면 코드를 재사용할 수 있다.
  <br>
  
- `유지보수 편의` & `코드의 신뢰성`<br>
  같은 코드 중복으로 인한 수정에 걸린는 시간을 함수 하나만 수정하며 단축시키고 사람의 실수를 줄인다.
  <br>
  
- `코드의 가독성`<br>
  함수는 *객체 타입의 값이다.<br>
  따라서, 함수의 이름을 정의할 수 있고, 적절한 함수 이름은 코드의 가독성을 높인다.

<br>

## 함수 리터럴
```JavaScript
var f = function add(x, y) {
  return x + y;
}
```

<br>

함수도 리터럴로 생성할 수 있고, `function`, `함수이름`, `매개변수목록`, `함수몸체`로 구성된다.<br>
함수 이름은 생략할 수 있고 생략시 `무명/익명 함수`라 한다.<br>
매개변수는 `순서에 따라` 할당되고, 변수와 같이 동일하게 취급된다.<br>

<br>

## 함수 정의

<br>

1. 함수 선언문
```JavaScript
function add(x, y) {
}
```

<br>

2. 함수 표현식
```JavaScript
var add = function() {
}
```

<br>

4. Function 생성자
```JavaScript
var add = new Function('x', 'y', 'return x = y');
```    
6. 화살표 함수
```JavaScript
var add = () => {};
```    

<br>

## 함수는 일급객체
```JavaScript
var add = function(x, y) {
  reutnr x + y;
}

console.log(add(1, 3)) // 4
```

함수는 `일급객체`이다 따라서 함수를 `값처럼` 자유롭게 사용할 수 있다는 의미이다.<br>
위 예제에서는 매개변수를 넘기는 것처럼 log()에 함수를 넘기고 있다.<br>
<br>

## 함수 생성 시점과 호이스팅

<br>

함수 선언문과 표현식은 함수의 `생성시점이 다르다`. 때문에 두 선언방법은 함수 호출시점에 대한 차이가 존재한다.

<br>

```JavaScript
console.log(add(2, 5)); // 7
console.log(sub(1, 3)); // TypeError

function add(x, y){
  return x + y;
}

var sub = function(x, y) {
  return x - y;
}
```

위 예제를 살펴보면 함수선언문은 `호이스팅`이 발생한 것처럼 보이고,<br>
표현식은 호이스팅이 발생하지 않은 것처럼 보인다.<br>
<br>
이는 서로의 `생성시점`이 다르기 때문이다.
<br>

- 함수선언문<br>
  런타임 이전에 함수 객체가 먼저 생성 후 함수이름과 동일한 이름의 `식별자를 암묵적`으로 생성, 함수를 할당한다.<br>
<br>
  
- 함수표현식<br>
  var sub = 는 호이스팅되어 undefined로 초기화되고 런타임 시점에 평가되어 함수객체가 할당된다.<br>
  따라서 위 예제는`undefined(1, 3)`를 호출한 것과 같다.<br>

## 함수 호출
함수의 매개변수는 함수 몸체에서만 참조할 수 있다. 즉 매개변수의 `스코프는 함수 몸체`이다.<br>
함수의 매개변수의 개수와 argument의 개수는 일치하지 않아도 된다. 빈 개수는 `undefined가 할당`된다.<br>
모든 매개변수는 `argument`에 보관된다.<br>

```JavaScript
function add(x, y) {
  console.log(x, y); // 1 2
  return x + y;
}

add(1, 2);
// 함수 몸체 내부에서만
console.log(x, y); // ReferenceError: x is not defined


// 나머지 매개변수 undefined
function mul(x, y) {
  console.log(x, y); // 1 undefined
}
mul(1);

// arguments 에 보관
function sub(x, y) {
  console.log(arguments); // 3, 2, 1
  return x - y;
}
sub(3, 2, 1); // 1

// 매개변수 기본값 지정

function test(x = '1') {
  return x;
}

test(); // 1
```

<br>

위 예제로 알 수 있는 다른 점은<br>
1. JS는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.<br>
2. JS는 동적타입언어로 매개변수의 타입을 `사전 정의 불가`하다.<br>

## 값에 의한 호출 / 참조에 의한 호출

<br>

- 값에 의한 호출<br>
  원시값은 `변경불가능한` 성질을 가진다.<br>
  따라서 인수는 값 자체가 복사되어 매개변수로 전달된다.<br>
  이 값을 변경해도 값 자체가 복사되어 원본 값을 `훼손하지 않는다`.<br>

<br>

- 참조에 의한 호출<br>
  객체는 `변경 가능한 성질`을 가진다.<br>
  즉 객체는 참조값이 복사되어 전달되고<br>
  참조 값을 변경 시 원본 값을 `훼손`한다.<br>

<br>

> 참조 값 복사에 의한 문제 해결
>  1. 옵저버 패턴을 통한 객체의 변경을 감지하고 이를 대응한다.
>  2. 객체를 `깊은 복사를` 통해 매개변수로 사용한다.

<br>

## 다양한 함수의 형태

<br>

1. 즉시 실행 함수<br>
   함수 정의와 동시에 즉시 호출되어 실행되는 함수
```JavaScript
(function () {
  // 실행
})();
```

<br>

2. 재귀함수<br>
   자기 스스로를 호출하는 함수,  반복되는 처리에 사용하기도 한다.

<br>

3. 중첩함수<br>
   함수 내부에 정의된 함수 외부함수와 내부함수로 구분된다.

<br>
   
```JavaScript
function outer () {
  var x = 1;

  function inner () {
    var y = 2;
  }
  inner();
}

outer();
```

<br>

4. 콜백함수
  - 매개변수를 통해 외부에서 내부로 전달받은 함수로
  - 함수를 매개변수로 받은 함수를 `고차함수`라 한다.
  - 콜백함수는 고차함수에 의해 실행된다.
  - 비동기 프로그래밍 시 순서를 보장하는 역할로 사용한다.
  - 그 유명한 `콜백 지옥`을 경험하게 한다.
