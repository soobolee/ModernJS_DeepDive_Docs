
# 타입 변환과 단축 평가

## 타입변환이란

자바스크립트의 모든 값은 타입이 있다. 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환 가능하다.

- 개발자의 `의도적인` 타입변환 -> `명시`적 타입변환, 타입 캐스팅
- 의도와는 `상관없는` 타입변환 -> `암묵`적 타입변환, 타입 강제변환

> 명시적이든 암묵적이든 타입변환은 기존 원시 값을 직접 변경하는 것은 아니다.
> 원시 값은 변경불가능한 값으로 새로운 메모리공간을 할당한다. 따라서 기존 할당 된 메모리는 `가비지 콜렉터`에 의해 `사라진`다.

> 따라서, 암묵적으로 드러나지 않는 타입변환은 예측하기 어려우므로 예측할 수 있는 좋은 코드를 작성해야 한다.

<hr>

## 암묵적 타입 변환

```JavaScript
// 문자열 연결 연산자로 작동하며 연산자를 문자열 값으로 암묵적 타입변환
 '10' + 2 // 102

// 곱하기로써 작동하며 문자열 10을 넘버타입으로 암묵적 타입변환
 5 * '10' // 50

// 부정 연산자로서 작동하며 0을 Boolean 타입으로 암묵적 타입변환
 !0 // true

// 논리 연산자가 들어가야할 자리에 숫자 1은 true로 암묵적 타입변환
 if ( 1 ) {} // true
```

<hr>

## 명시적 타입변환

```JavaScript
 String(1) // '1'
 String(NaN) // 'NaN'
 String(true) // 'true'
 (1).toString(); // '1'

 Number('0'); // 0
 Number('false'); // 0
 parseInt('0'); // 0
 +'0'; // 0
 '0' * 1; // 0

 Boolean('dsf'); // true
 Boolean(''); // false
 Boolean(Infinity) // true
 Boolean({}); // true
 Boolean([]); // true
```

<hr>

## 단축평가

논리 연산자를 이용해 평가식의 평가 정도를 `단축(생략)` 시킬 수 있다.
<br><br>
논리 연산자의 종류로는

- `|| (논리함)` : 하나라도 true면 true반환
- `&& (논리곱)` : 피연산자가 모두 true여야 true반환
이 있다.

위에 적힌 논리 연산자의 작동방식을 알면 단축평가에 대해서도 쉽게 이해 가능하다<br>
<br>
`|| (논리합)`
- 하나라도 truthy이면 truthy값을 반환한다. 즉, 피연산자중 가장 앞에 있는 truthy값을 반환하고 truthy가 없다면 가장 뒤에 값을 반환
  
`&& (논리곱)`
- 하나라도 falsy이면 falsy값을 반환한다. 즉, 피연산자중 가장 앞에 있는 falsy값을 반환하고 falsy가 없다면 가장 뒤에 값을 반환

```JavaScript
 // || 는 가장 먼저 truthy한 값을 반환하고 평가식을 생략한다.
 'cat' || 'dog' // cat
 false || 'dog' // dog
 'cat' || false // cat

// && 는 가장 먼저 falsy한 값을 반환하고 평가식을 생략한다.
 'cat' && 'dog' // dog
 false && ’dog' // false
 'cat' && false // false
```

> 단축평가를 이용한 에러 없이 객체 값 확인
> ```JavaScript
>  var elem = null;
>
>  var val = elem && elem.value;
> ```
> 단축평가 없이 바로 elem.value를 참조했다면 참조에러가 났겠지만 단축평가를 이용해 뒤에 평가식을 `생략`시켜 에러를 회피했다.

<hr>

## 옵셔널 체이닝 연산자

위에 단축평가를 이용한 에러 회피보다 더 나은 방식으로 `?.` 로 사용하고, 좌항의 피연산자가 `null` 또는 `undefined`일 경우 undefined를 `반환`하고 에러를 뿜지 않는다.

```JavaScript
 var elem = null;

 var val = elem?.value;
 console.log(val); // undefined
```

<hr>

## null 병합 연산자

null 변합 연산자는 `??` 로 사용하고, 피연산자가 `null` 또는 `undefined`일 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항을 `반환`한다.

```JavaScript
 var elem = null;

 var val = elem ?? 'no value';
 console.log(val); // 'no value'
```

변수에 `기본값을 설정할 때 매우 유용`하다.<br>
<br>
#### `필자`도 서버에서 받아온 `Json 객체`에서 값을 꺼낼 때 null 변합 연산자를 이용하여 예기치 못하게 서버에서 값이 오지 않았을 때 나올 에러를 `예외처리`하고 있다.
