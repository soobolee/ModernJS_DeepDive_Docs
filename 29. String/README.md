

# String 

표준 빌트인 객체인 String 객체는 `생성자 함수 객체`다.<br>
따라서, new 연산자와 함께 호출하여 String 인스턴스를 생성가능하다.<br><br>

```JavaScript
const strObj = new String();

console.log(strObj); // String{length : 0, [[PrimitiveValue]] : ""}
```

new 연산자와 함께 인수를 전달하지 않고 생성하면, [[StringData]] 내부슬롯에 빈 문자열을 `할당`한 String 래퍼객체를 생성한다.<br><br>

```JavaScript
const strObj = new String('leesoobo');

console.log(strObj); // String{0: 'l', 1: 'e', 3: 'e', ..., length : 8, [[PrimitiveValue]] : "leesoobo"}
```
new 연산자와 함께 인수를 전달하고 생성하면, 인수로 전달받은 문자열을 할당한 래퍼 객체를 생성한다.<br><br>

## 문자열과 불변성

String 래퍼 객체는 배열과 마찬가지로 위 예제처럼 length 프로퍼티와 인덱스를 <br>
나타내는 숫자 형식의 문자열을 프로퍼티 키로 각 문자를 프로퍼티 값으로 갖는<br>
`유사배열객체` 이면서 `이터러블 객체`이다.<br><br>

따라서, 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.<br><br>

그러나 문자열은 원시 값이므로 변경이 불가능하다.<br><br>

```JavaScript
let str = 'hello';

str[3] = 'L';

console.log(str);
// hello
```

## String을 이용한 명시적 타입변환

String 생성자 함수 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로<br>
`강제 변환`하여 문자열로 할당하여 반환한다.<br><br>

이를 이용하여 명시적으로 `문자열을 반환`한다.<br><br>

```JavaScript
String(1)    // '1'
String(NaN)  // 'NaN'
String(true) // 'true'
String('hi') // 'hi'
```

## String의 length 프로퍼티

String 래퍼 객체는 배열과 마찬가지로 `length 프로퍼티`를 가진다.<br>
인덱스를 나타내는 숫자를 프로퍼티키로 각 문자를 값으로 가지기 때문에<br>
`유사배열` 객체이다.<br><br>
```JavaScript
let str = 'hello';

str.length; // 5
```

## String 메서드

1.  String.prototype.indexOf
2.  String.prototype.search
3.  String.prototype.includes
4.  String.prototype.startsWith
5.  String.prototype.endsWith
6.  String.prototype.charAt
7.  String.prototype.substring
8.  String.prototype.slice
9.  String.prototype.toUpperCase
10. String.prototype.toLowerCase
11. String.prototype.trim
12. String.prototype.repeat
13. String.prototype.replace
14. String.prototype.split

<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String">String 자세히 보러가기</a>










