

# String 

표준 빌트인 객체인 String 객체는 생성자 함수 객체다.
따라서, new 연산자와 함께 호출하여 String 인스턴스를 생성가능하다.

```JavaScript
const strObj = new String();

console.log(strObj); // String{length : 0, [[PrimitiveValue]] : ""}
```

new 연산자와 함께 인수를 전달하지 않고 생성하면, [[StringData]] 내부슬롯에 빈 문자열을 할당한 String 래퍼객체를 생성한다.

```JavaScript
const strObj = new String('leesoobo');

console.log(strObj); // String{0: 'l', 1: 'e', 3: 'e', ..., length : 8, [[PrimitiveValue]] : "leesoobo"}
```
new 연산자와 함께 인수를 전달하고 생성하면, 인수로 전달받은 문자열을 할당한 래퍼 객체를 생성한다.

## 문자열과 불변성

593페이지 맨위

## String을 이용한 명시적 타입변환
593 중간

## String의 length 프로퍼티
594페이지 맨위

## String 메서드




