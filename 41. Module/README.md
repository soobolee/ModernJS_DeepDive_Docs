

# 모듈

애플리케이션을 구성하는 개별적 요소로, 재사용 가능한 코드 조각을 의미한다.<br>
일반적으로, 모듈은 기능을 기준으로 `파일 단위`로 분리한다.<br><br>

따라서 모듈은 자신만의 `파일 스코프`를 가져야 한다.<br><br>

- 즉, `캡슐화` 되었으므로 다른 모듈에서 접근할 수 없다.<br><br>

- `export` : 모듈이 공개가 필요한 자신에 한정하여 `명시적`으로 `선택적 공개`가 가능하다.<br>
- `import` : 모듈 사용자는 모듈이 export한 자산 중 `일부` 또는 `전체`를 선택해 자신의 스코프 내로 불러들여 `재사용`할 수 있다.<br>


> 모듈은 `재사용성`이 좋아서 개발 효율과 유지보수성을 높일 수 있다.<br>

## ES6 모듈

ES6 에서 클라이언트 사이드 JS에서 동작하는 모듈 기능을 추가했다.<br><br>

- IE를 제외한 모든 브라우저에서 사용 가능<br>

```JavaScript
// 사용법
<script type="module" src="app.mjs"></script>

// ESM임을 명확히하기 위해 ESM의 파일 확장자는 .mjs 를 사용할 것을 권장
// ESM은 기본적으로 strict mode가 적용 된다.
```

> ESM은 파일 자체의 독자적인 모듈스코프를 제공하기 때문에 모듈 내에서 var 로 선언한 변수는 전역변수나 window객체의 프로퍼티가 아니다.

<br>

## export 키워드

모듈 내에서 선언한 식별자를 외부에 공개해서 `다른 모듈`이 `재사용`하기 위해 사용하는 키둬드<br><br>

- export 키워드는 선언문 앞에 위치<br>
- 변수, 함수, 클래스 등 모든 식별자에 export 사용 가능<br>
- 변수를 모두 모아 하나의 객체로 만들어 export 적용 가능<br><br>

```JavaScript
const sum = (a, b) => a + b;
const sub = (a, b) => a - b;
const mul = (a, b) => a * b;

// 하나의 객체로 export -> 묶어서 공개
export { sum, sub, mul };
```

## import 키워드

다른 모듈에서 공개[export]한 식별자를 자신의 모듈 스코프 내에서 `사용`하려고 `로드`할 때 사용<br><br>

- 다른 모듈이 export한 식별자 이름으로 import해야 한다.<br>
- ESM의 경우 파일 확장자를 생략할 수 없다.<br>
- export 처럼 하나의 객체로 묶어 한 번에 import 할 수 있다.<br><br>

```JavaScript
// as 뒤에 lib 라는 이름의 객체로 묶어서 사용하겠다는 것, 즉, 식별자 이름 변경 가능
// * : 모든 자산
import * as lib from "/lib.mjs";

console.log(lib.sum(1, 2));
console.log(lib.sub(1, 2));
console.log(lib.mul(1, 2));
```

모듈에서 하나의 값만 export 한다면 default 키워드를 사용할 수 있다.<br>
기본적으로 이름 없이 하나의 값만 export한다.<br>

- default 사용 시 `let`, `const`, `var` 키워드 사용 `불가` <br>
```JavaScript
// lib.mjs
export default x =< x * x;


// app.mjs
import square from "./lib.mjs";
```





















