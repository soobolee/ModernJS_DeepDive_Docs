
# 클로저란

클로저는 앞서 20 챕터에서 설명한 `실행 컨텍스트`에 대한 `사전지식`이 필요하다.<br>
클로저는 JS 고유의 개념이 아닌 함수를 `일급 객체`로 취급하는 언어에서 사용되는 개념이다.<br><br>

클로저는 주변 상태에 대한 참조와 함께 묶인 함수의 조합으로 즉, 내부 함수에서 외부 함수의 범위에 대한 접근을 제공한다.<br>
JS에서 `클로저`는 `함수`가 `생성될 때마다 생성`된다.<br>

<hr>

# 렉시컬 스코프

클로저를 설명하기에 앞서 렉시컬 스코프의 개념을 알고가야한다.<br>

```JavaScript
const x = 1;

fiunction outerF() {
  const x = 10;

  function innerF() {
    console.log(x); // 10
  }

  innerF();
}

outerF();
```

outerF 함수 내부에서 중첨함수가 정의되고 호출되었다. 이때 중첩함수는 상위 스코프인 outerF함수의 X 변수에 접근이 `가능`하다<br>
따라서 10이 출력된다.<br><br>

```JavaScript
const x = 1;

function outerF() {
  const x = 10;
  innerF();
}

function innerF() {
  console.log(x);
}

outerF(); // 1
```

만약 innerF함수가 outerF함수 내부에서 정의된 중첩함수가 아니라면 innerF에서의 X 변수는 전역변수를 가리킨다.<br><br>

이 현상의 이유는 자바스크립트가 ***렉시컬 스코프***를 따르는 프로그래밍 언어이기 때문이다.<br><br>

> - 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 `어디에 정의`했는지에 따라 상위 스코프를 결정한다. 이것이 `렉시컬 스코프`

<br>

# 클로저

```JavaScript
const x = 1;

function outerF() {
  cosnt x = 10;
  const innerF = function() { console.log(x) };

  return innerF;
}

const innerFunc = innerF();

innerFunc(); // 10;
```

함수 outerF는 innerF를 반환하고 콜스택에서 팝되었다.<br> 
즉, outerF의 변수 `x` 또한 더이상 유효하지 않고 스코프 어디에서도 찾을 수 없다.<br><br>

`그러나` 위 코드에서 실행결과는 outerF 내부의 변수 `x`를 가리킨다.<br>
이처럼 자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, <br>
외부 함수 밖에서 내부 함수가 호출되더라도 외부 함수의 지역변수에 접근할 수 있는 이러한 중첩함수를 ***클로저***라고 부른다.<br><br>

즉, `클로저`는 자신이 생성될 때의 환경을 기억한다.<br>
이 말은 중첩함수는 `내부슬롯 [[Environment]]`에 아우터함수를 `참조`하고 있으며, <br>
가비지 콜렉터는 누군가 참조하고 있는 메모리 공간을 `함부로 해제하지 않는다.`<br><br>

즉, outerF함수의 렉시컬 환경은 함수 생존여부와 관계없이 유지된다.<br><br>

## 클로저의 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.<br>
즉, 상태가 의도치 않게 변하지 않도록 상태를 `은닉`하고 특정함수에게만 변경을 허락한다. -> **캡슐화**라고도 한다.<br><br>

```JavaScript
const increase = (function() {
  let num = 0;

  return function() {
    return ++num;
  }
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 예제에서 increase함수가 호출되면 즉시실행함수로 `생성되자마자 소멸`되면서 전달된다 따라서, num은 `정보은닉`상태가 된다.<br>
따라서 은닉된 num변수에는 내부함수만 접근이 가능하다.<br><br>

- 위 예제를 생성자 함수로<br>

```JavaScript
function Counter() {
  // this. 를 사용하면 인스턴스에 바인딩되므로 public이 되어버림
  let count = 0;

  this.increase = function() {
    return ++count;
  }
}

const counter = new Counter();

console.log(counter.increase()); // 1
```





















