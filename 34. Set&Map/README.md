
# Set

`Set` 객체는 중복되지 않는 `유일한 값`들의 `집합`이다. 배열과 유사하지만 다음과 같은 차이가 있다.<br><br>

|구분|객체|Set|
|------|---|---|
|동일한 값의 중복을 허용|O|X|
|요소 순서에 의미|O|X|
|인덱스로 요소 접근|O|X|

- Set 객체 생성<br>
  `생성자 함수`로 생성<br>
  Set생성자 함수는 이터러블을 인수로 전달 받아 Set 객체 생성<br><br>

  - 중복된 값은 Set객체에서 저장되지 않는다.<br>
  - 중복을 허용하지 않기 때문에 배열의 `중복제거`에 주로 사용<br><br>

```JavaScript
// Set
const set1 = new Set([1, 2, 2, 3]);
console.log(set1); // Set(3) { 1, 2, 3 }

const set2 = new Set("Javascript");
console.log(set2); // Set(9) { 'J', 'a', 'v', 's', 'c', 'r', 'i', 'p', 't' }

// 중복된 요소 제거 -> 배열
const uniq = (arr) => arr.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]

// 중복된 요소 제거 -> Set 객체
const uniq = (arr) => [...new Set(arr)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]
```

<br>

- Set 메서드<br>

```JavaScript
const set1 = new Set([1, 2, 2, 3]);

// size 프로퍼티
set1.size // 3

// add 메서드
set1.add(4) // 1, 2, 3, 4

// has 메서드
set1.hase(3); // true

// delete 메서드
set1.delete(3); // 1, 2, 4

// forEach 메서드
// v1 -> 현재 순회 중인 요소
// v2 -> 현재 순회 중인 요소
// self -> 현재 순회 중인 객체자신
set1.forEach((v1, v2, self) => console.log(v1, v2, self));
// 1 1 Set(3) { 1, 2, 4 }
// 2 2 Set(3) { 1, 2, 4 }
// 4 4 Set(3) { 1, 2, 4 }

// clear 메서드
let result = set.clear(); // set(0) {}
```

<br>

# Map

`Map` 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. Map 객체는 일반 객체와 유사하지만 다음과 같은 차이가 있다.<br><br>

|구분|객체|Map|
|------|---|---|
|키로 사용할 수 있는 값|문자열 또는 심벌값|객체를 포함한 모든 값|
|이터러블|X|O|
|요소개수 확인|Object.keys(obj).length|Map.prototype.size|

- Map 객체 생성<br>
  `생성자 함수`로 생성<br>
  Map 생성자 함수는 이터러블을 전달받아 생성<br>
  인수로 전달한 이터러블에 `중복된 키가` 있다면 `덮어 씌인다.`<br>
  - 즉 같은 키가 존재할 수 없다.<br><br>

```JavaScript
const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
  ["key2", "value3"],
]);
console.log(map1); // Map(2) { 'key1' => 'value1', 'key2' => 'value3' } // 덮어씌워짐

const map2 = new Map([1, 2]); // 키와 값이 있는 이터러블이어야 한다. 에러남
```

<br>

- Map 메서드<br>

```JavaScript
const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
  ["key2", "value3"],
]);

// size 프로퍼티
map1.size = 10; // 무시
map1.size // 3

// set 메서드
map1.set('key3', 'value4'); // key1, value1, ket2 valye3, key3 value4

// get 메서드
map1.get('key1'); // value1

// has 메서드
map1.has('key1'); // true

// delete 메서드
map1.delete('key3'); // Map(2) {key1 : value1, key2 : value3}

// forEach 메서드
// 첫 번째 인수 → 현재 순회 중인 요소 값
// 두 번째 인수 → 현재 순회 중인 요소 키
// 세 번쨰 인수 → 현재 순회 중인 Map객체 자신
map1.forEach((v, k, self) => console.log(v, k, self));
// value1 key1 Map(2) ... 
// value3 key2 Map(2) ...

// keys 메서드
// Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
for(const key of map1.keys()) {
  console.log(key);
}
// key1
// key2

// values 메서드
// Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
for(const key of map1.values()) {
  console.log(key);
}
// value1
// value3

// entries 메서드
// Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
for(const key of map1.entries()) {
  console.log(key);
}
// {key1 : value2}
// {key2 : value3}

// clear 메서드
map1.clear(); // Map(0) {}
```
