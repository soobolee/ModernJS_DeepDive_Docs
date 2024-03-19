
# Date

표준빌트인 객체 Date는 날짜와 시간[연, 월, 일, 시, 분, 초, 밀리초]를 위한 메서드를 제공하는 <<br><br>
빌트인 객체이면서 생성자 함수다.<br><br>

UTC 는 국제 표준시로 그리니치 평균시[GMT]와 밀리초 차이만 나기 때문에 일상에선 혼용하여 사용하지만<br>
기술적으로 UCT를 사용한다.<br><br>

KST 는 UTC에 9시간을 더한 시간으로 즉, 한국 표준시이다.<br><br>

## Date 생성자 함수

Date는 생성자 함수다. Date 생성자 함수로 생성한 객체는 내부적으로 현재 날짜와 시간을 나타내는 정수값을 가진다.<br>
현재 날짜나 시간이 아닌 특정 날짜, 시간을 원한다면 Date 생성자 함수에 명시적으로 해당 날짜와 시간을 인수로 보낸다.<br>

- new Date();<br>
  인수 없이 호출 시 현재 날짜와 시간을 가지는 Date 객체 반환<br>
  내부적으로 날짜와 시간을 나타내는 정수값을 가지지만 Date객체를 콘솔에 찍어보면 String으로 출력<br><br>
  ```JavaScript
  new Date(), typeof new Date(); // Tue Jan 04 2022 22:43:51 GMT+0900 (한국 표준시) -> object
  Date(), typeof Date(); // Tue Jan 04 2022 22:44:45 GMT+0900 (대한민국 표준시) -> string
  ```

- new Date(milliseconds)<br>
  인수로 숫자 타입의 밀리세컨드를 전달하면 UTC[1970년 1월 1일 00:00:00]을 기준으로 인수로 전달된<br>
  밀리세컨드만큼 경과된 시간을 Date객체에 담아 반환한다.<br><br>
  ```JavaScript
  new Date(86400000); -> 1970년 1월 2일

  1s = 1,000ms
  1m = 60s * 1,000ms
  1h = 60m * 60,000ms
  1d = 24h * 3,600,000ms
  ```
  
- new Date(dateString)<br>
  인수로 전달된 시간과 날짜를  나타내는 Date객체를 반환<br>
  - 전달하는 문자열은 날짜로 해석 가능한 형식이어야 한다.<br><br>

- new Date(year, month[,day, hour, minute, second, milliseconds])<br>
  Date생성자 함수에 연, 일, 시, 분, 초, 밀리초 를 의미하는 숫자를 인수로 전달하면<br>
  지정된 날짜와 시간을 나타내는 Date객체를 반환<br>
  - 연과 월은 필수이다.<br><br>
  ```JavaScript
  console.log(new Date(2024, 2)); // Sun Mar 01 2024 00:00:00 GMT+0900 (한국 표준시)
  console.log(new Date(2024, 2, 26, 10, 00, 00, 0)); // Thu Mar 26 2024 10:00:00 GMT+0900 (한국 표준시)
  ```


## Date 정적 메서드

1. Date.now <br>
2. Date.parse<br>
3. Date.UTC<br>

## Date 프로토타입 메서드

1. Date.prototype.getFullYear<br>
2. Date.prototype.setFullYear<br>
3. Date.prototype.getMonth<br>
4. Date.prototype.setMonth<br>
5. Date.prototype.getDate<br>
6. Date.prototype.setDate<br>
7. Date.prototype.getDay<br>
8. Date.prototype.getHours<br>
9. Date.prototype.setHours<br>
10. Date.prototype.getMinutes<br>
11. Date.prototype.setMinutes
12. Date.prototype.getSeconds
13. Date.prototype.setSeconds
14. Date.prototype.getMilliseconds
15. Date.prototype.setMilliseconds
16. Date.prototype.getTime
17. Date.prototype.setTime
18. Date.prototype.getTimezoneOffset
19. Date.prototype.toDateString
20. Date.prototype.toTimeString
21. Date.prototype.toISOString
22. Date.prototype.toLocaleString


<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date">Date 자세히 보러가기</a>









