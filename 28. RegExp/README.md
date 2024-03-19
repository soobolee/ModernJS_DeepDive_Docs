

# RegExp

## 정규표현식

일정한 패턴을 가진 문자열의 잡합을 표현하기 위해 사용하는 형식 언어이다.<br><br>

패턴매칭 기능을 제공하여, 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.<br><br>

패턴매칭 기능을 이용하여<br>
 - `검색`, `추출`, `치환`하거나<br>
 - 조건문 없이 패턴을 파악하고, 테스트할 수 있다.<br><br>

그러나 정규표현식은 여러가지 기호를 사용하고, 공백을 허용하지 않아서 `가독성에 좋지 않다`.<br><br>

## 정규표현식의 생성

정규표현식 리터럴과 RegExp생성자 함수를 사용하는 두 가지 방법을 이용해 생성한다.<br>
일반적으로는 리터럴을 사용해 생성한다.<br><br>

```JavaScript
/regexr/i

// [/]       시작기혼
// [regext]  패턴
// [/]       종료기호
// [i]       플래그
```

## 플래그의 종류

정규표현식의 검색 방식을 설정하기 위해 사용한다.<br>
총 6개가 존자하며 주로 쓰이는 것은 i, g, m 이다.<br><br>

|플래그|의미|설명|
|------|---|---|
|i|Ignore case|대소문자를 구분하지 않고 패턴 검사|
|g|Global|대상 문자열 내에서 패턴과 일치하는 모든 문자열 검사|
|m|Multi Line|문자열이 행이 바뀌더라도 패턴을 계속 검사|

## 패턴

패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표기한다.<br><br>

- 문자열 검색 패턴<br>
  정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열 검색<br>
  ```JavaScript
  const target = "Is this all there is?";

  // 문자열 검색 + i 플래그 대소문자 X
  console.log(target.match(/is/i).l); // [ 'Is', index: 0, input: 'Is this all there is?', groups: undefine
  
  // 문자열 검색 + g 플래그 전역 검색
  console.log(target.match(/is/g)); // [ 'is', 'is' ]
  
  // 문자열 검색 + i 플래그 + g 플래그
  console.log(target.match(/is/gi)); // [ 'Is', 'is', 'is' ]
  ```

- 임의의 문자열 검색<br>
  `.`은 임의의 문자 한 개를 의마한다. ...을 3개 연속 사용하면 3자리 문자열과 매치한다.<br>
  ```JavaScript
  const target = 'Is this all there is?';

  const regExp = /.../g;
  
  target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
  ```

- 반복 검색<br>
 `{m, n}`은 패턴이 `최소 m` 번 `최대 n` 번 반복되는 문자열을 의마한다. 콤마 뒤에 공백이 존재해선 안된다. 작동X<br>
  m과 n의 숫자가 같다면 `{n}` 으로 축약해서 사용할 수 있다.<br>
  ```JavaScript
  const target = 'A AA B BB Aa Bb AAA';

  const regExp = /A{1,2}/g;

  target.match(regExp); // ["A", "AA", "A", "AA", "A"]
  ```

- OR 검색 <br>
  `|` 은 or의 의미를 갖는다. /A|B/g; 는 A 또는 B를 의미한다.<br>
  ```JavaScript
  const target = 'A AA B BB Aa Bb AAA';

  const regExp = /A|B/g;

  target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
  ```

- NOT 검색<br>
  [...] 내의 `^` 는 NOT을 의미한다. [^0-9]는 숫자를 제외한 문자를 의미한다.<br>
  ```JavaScript
  const target = 'AA BB 12 Aa Bb';

  const regExp = /[^0-9]+/g;

  target.match(regExp); // ["AA BB ", " Aa Bb"]
  ```

- 시작 위치로 검색<br>
  [...] 밖의 `^` 는 문자열의 시작을 의미한다.<br>
  ```JavaScript
  const target = 'https://leesoobo.com';

  const regExp = /^https/;

  regExp.test(terget); true
  ```

- 마지막 위치로 검색<br>
  `$`는 문자열의 마지막을 의미한다.<br>
  ```JavaScript
  const target = 'https://leesoobo.com';

  const regExp = /com$/;

  regExp.test(terget); true  
  ```

## RegExp 메서드

1. RegExp.prototype.exec<br>
   인수로 받은 정규식으로 패턴을 검사하여 매칭 결과를 `배열`로 반환한다.<br>
   - `g` flag를 세우면 첫 번째 매칭 결과만 배열로 반환<br><br>
     
2. RegExp.prototype.test<br>
   인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 결과를 `boolean`으로 반환한다.<br><br>

3. RegExp.prototype.match<br>
   인수로 전달받은 정규식과의 매칭 결과를 `배열`로 반환한다.<br>
   - `g` flag를 세우면 전역 매칭 결과를 배열로 반환<br><br>


## 자주 사용하는 정규표현식

```JavaScript
// 특정 단어로 시작하는지 검사
const http = "http://example.com";
const https = "https://example.com";
const testUrl = "httpss://example.com";

const regExp = /^https?:\/\//;

console.log(regExp.test(http)); // true
console.log(regExp.test(https)); // true
console.log(regExp.test(testUrl)); // false
```

```JavaScript
// 특정 단어로 끝나는지 검사
const fileName1 = "index.html";
const fileName2 = "index.js";

const regExp = /html$/;

console.log(regExp.test(fileName1)); // true
console.log(regExp.test(fileName2)); // false
```

```JavaScript
// 숫자로만 이뤄진것 검사
const target = "12345";
const regExp = /^\d+$/;

console.log(regExp.test(target)); // true
```

```JavaScript
// 하나 이상의 공백으로 시작하는지 검사
const targets = [" Hi!", "Hi! ", "Hi!"];
const regExp = /^[\s]+/;

targets.forEach((target) => console.log(regExp.test(target)));
// " Hi!" => true
// "Hi! " => false
// "Hi!"  => false
```

```JavaScript
// 아이디 사용여부 검사
const userIds = ["id123", "id123$", "id_123", "id1234123400"];
const regExp = /^[A-Za-z0-9]{4,10}$/;

userIds.forEach((userId) => console.log(regExp.test(userId)));
// id123        => true
// id123$       => false
// id_123       => false
// id1234123400 => false
```

```JavaScript
// 이메일 형식 검사
const emails = ["email@email.com", "email#email.com", "email@email.hack", "em<sricpt>@email.com"];
const regExp = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-z]{2,3}$/;

emails.forEach((email) => console.log(regExp.test(email)));
// email@email.com      => true
// email#email.com      => false
// email@email.hack     => false
// em<sricpt>@email.com => false
```

```JavaScript
// 핸드폰 번호 형식 검사
const phoneNumbers = ["010-1234-5678", "02-1234-5678", "032-123-4567", "02-123-4567", "010:1234:5678"];

phoneNumbers.forEach((phoneNumber) => {
  console.log(/^\d{2,3}-\d{3,4}-\d{4}$/.test(phoneNumber));
});
// 010-1234-5678 => true
// 02-1234-5678  => true
// 032-123-4567  => true
// 02-123-4567   => true
// 010:1234:5678 => false
```

```JavaScript
// 특수 문자 포함여부 검사
const target1 = "abc<123";

console.log(/[^A-Za-z0-9]/gi.test(target1)); // true

// 특수 문자 확인 시 제거하기 ( accessor method, 원본 데이터 변경 X )
console.log(target1.replace(/[^A-Za-z0-9]/gi, "")); // abc123
console.log(target1); // true
```





