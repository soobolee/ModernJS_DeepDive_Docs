
# Number

표준 빌트인 객체인 `Number 객체`는 `생성자함수 객체`다.<br>
따라서, new 연산자와 함께 호출하여 Number `인스턴스`를 생성할 수 도 있다.<br><br>

```JavaScript
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]] : 0}
```

<br>
Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.<br><br>

```JavaScript
const numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]] : 10}
```

숫자가 아닌 값을 전달하면 인수를 `강제로 숫자로 변환`한 후 변환된 숫자를 할당한다.<br>
인수를 숫자로 변경 시킬 수 없다면 NaN이 포함된 Number래퍼 객체를 생성한다.<br><br>

new 연산자 없이 Number를 호출하면 Number인스턴스가 아닌 일반 원시 `숫자타입을 반환`한다.<br>
이를 이용하여 `명시적 타입변환`을 하기도 한다.<br><br>

```JavaScript
Number('0');    // 0
Number('-3');   // -3
Number('0.14'); // 0.14

Number(true);  // 1
Number(false); // 0

```

<br>

## Number 프로퍼티<br>
1.  Number.EPSILON<br>
2.  Number.MAX_VALUE<br>
3.  Number.MIN_VALUE<br>
4.  Number.MAX_SAFE_INTEGER<br>
5.  Number.MIN_SAGE_INTEGER<br>
6.  Number.POSITIVE_INFINITY<br>
7.  Number.NEGATIVE_INFINITY<br>
8.  Number.NaN<br>

## Number 메서드<br>
1.  Number.isFinite<br>
2.  Number.isInteger<br>
3.  Number.isNaN<br>
4.  Number.isSafeInteger<br>
5.  Number.prototype.toExponential<br>
6.  Number.prototype.toFixed<br>
7.  Number.prototype.toPrecision<br>
8.  Number.toString<br>

<br>
<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number">Number 프로퍼티, 메서드 자세히 보기</a>
