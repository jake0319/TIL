# 맵과 셋
- 객체 : 키가 있는 컬렉션을 저장함
- 배열 : 순서가 있는 컬렉션을 저장함
- `하지만 현실 세계를 반영하기엔 이 두 자료구조 만으론 부족해서 Map과 Set이 등장하였습니다`

<br>

## Map
- 키가 있는 데이터를 저장함(`객체`와 유사)
- `단, 키에 다양한 자료형들을 허용`( 키값으로 string,Symbol만을 허용하는 객체와의 차이점이 있음 )

<br>

### Map 프로퍼티 (`set(k,v) , get(k) , has(k) , delelte(k) , clear() , .size`)
- `new Map()` - 맵을만듬
- `map.set(key,value)` - key를 이용해 value를 저장
- `map.get(key)` - key에 해당하는 value를 반환 (key가 없다면 undefined반환)
- `map.has(key)` - key가 존재하면 true, 없다면 false
- `map.delete(key)` - key에 해당하는 값을 삭제
- `map.clear()`- 맵 안의 모든 요소를 제거함
- `map.size`- map의 요소의 개수를 반환

<br>

예시:
```js
let map = new Map();
///key -value로 set저장 
map.set('1', 'str1');   // 문자형 키
map.set(1, 'num1');     // 숫자형 키
map.set(true, 'bool1'); // 불린형 키

// 객체는 키를 문자형으로 변환(문자 오버라이딩)한다는 걸 기억하고 계신가요?
// 맵은 키의 타입을 변환시키지 않고 그대로 유지합니다.
// 1과 '1'은 같은 key가 아님
alert( map.get(1)   ); // 'num1'  
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3 :요소의 개수
```

<br>

__주의사항__ 

<br>

- `cf.)`map은 객체가 아니므로 `map[key]` 와같이 사용하면 안됩니다.
>map[key] = 2로 값을 설정하는 것 같이 map[key]를 사용할 수 있긴 합니다. 하지만 이 방법은 map을 일반 객체처럼 취급하게 됩니다. 따라서 여러 제약이 생기게 되죠.  
`map을 사용할 땐 map전용 메서드 set, get 등을 사용해야만 합니다.`

<br>

- map은 객체를 key값으로 사용할 수 있습니다!
- 객체를 키로 사용하면 객체변수가 문자열로 오버라이딩
```js
let john = { name: "John" };

let visitsCountObj = {}; // 객체를 하나 만듭니다.

visitsCountObj[john] = 123; // 객체(john)를 키로 해서 객체에 값(123)을 저장해봅시다.

// 원하는 값(123)을 얻으려면 아래와 같이 키가 들어갈 자리에 `object Object`를 써줘야합니다.
alert( visitsCountObj["[object Object]"] ); // 123
```

<br>

### map이 키를 비교하는 방식
- `SameValueZero` 알고리즘 
- 원래 NaN 비교연산자로 비교가 불가(같은 NaN조차도 같다고 판별하지 않기때문)
>맵은 SameValueZero라 불리는 알고리즘을 사용해 값의 등가 여부를 확인합니다. 이 알고리즘은 일치 연산자 ===와 거의 유사하지만, NaN과 NaN을 같다고 취급하는 것에서 일치 연산자와 차이가 있습니다. 따라서 `맵에선 NaN도 키로 쓸 수 있습니다.`

<br>

## 맵의 요소에 반복 작업하기
- 다음 세 가지 메서드를 사용해 맵의 각 요소에 반복 작업을 할 수 있습니다.

1. map.keys() – 맵객체의 `요소들의 키를 모은 이터러블 객체`를 반환.
  - 이터러블: Symbol.iterator메서드가 구현되어 for..of으로 순회가능한 객체
  - ex: 배열,문자열,맵,셋과 같은 js내장객체 혹은 Symbol.iterator메서드(iterator객체를 반환)하는데,
  - 이터레이터 객체는 next()메서드를 갖고있음 
    - `arr[Symbol.iterator]().next()`와 같은 형태로 다음 요소를 참조 (for..of메서드 내부구현)
      - `arr[Symbol.iterator]() => 이터레이터객체(Symbol.iterator메서드 리턴값) =>이터레이터객체.next()`

```js
const myIterable = {
  const myIterable = {
  data: [1, 2, 3],
  [Symbol.iterator]() {  
    let index = 0;
    return {
      next: () => {
        if (index < this.data.length) {
          return { value: this.data[index++], done: false };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};
for (const value of myIterable) {
  console.log(value);
}
}
```

```js
let recipeMap = new Map([ //이터러블을 map객체로 
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// 키(vegetable)를 대상으로 순회합니다.
for (let vegetable of recipeMap.keys()) { //맵객체의 요소들의 키를 모은 배열을 반환
  alert(vegetable); // cucumber, tomatoes, onion
}

// 값(amount)을 대상으로 순회합니다.
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회합니다.
for (let entry of recipeMap) { // recipeMap.entries()와 동일합니다.
  alert(entry); // cucumber,500 ...
}
```
2. map.values() – 각 요소의 값을 모은 이터러블 객체를 반환합니다.
3. map.entries() – 요소의 [키, 값]을 한 쌍으로 하는 이터러블 객체를 반환합니다. 이 이터러블 객체는 for..of반복문의 기초로 쓰입니다.
- `맵은 값이 삽입된 순서대로 순회를 실시합니다. 객체가 프로퍼티 순서를 기억하지 못하는 것과는 다릅니다.`

<br

### Object.entries: 객체를 Map으로 바꾸기
- Object.entries()는 객체의 모든 key-value쌍을 요소로 갖는 `배열`을 리턴함.
```js
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));

alert( map.get('name') ); // John
```
- `Object.entries(obj)`를 사용해 객체 obj를 배열 `[ ["name","John"], ["age", 30] ]`로 바꾸고, 이 배열을 이용해 새로운 맵을 만듦.

<br>

### Object.fromEntries(key-value쌍 요소를 가진 이터러블) : Map을 객체로 바꾸기 
- Map의 key-value쌍 요소들을 객체로 바꾸어줌 (object.entries()의 반대기능)
```js
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// now prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange); // 2
```

<br>

- 자료가 맵에 저장되어있는데, 서드파티 코드에서 자료를 객체형태로 넘겨받길 원할 때 이 방법을 사용할 수 있습니다.
```js
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);
// map.entries()는 key-value쌍 배열요소를 가진 이터러블을 반환
let obj = Object.fromEntries(map.entries()); // 맵을 일반 k-v프로퍼티를 가진 객체로 변환 (*)

// 맵이 객체가 되었습니다!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```


<br>
<br>
<br>

# SET
- `중복을 허용하지 않는 값을 모아놓은 특별한 컬렉션으로, 키가 없는 값이 저장됨.`

<br>

## SET프로퍼티
- new Set(iterable) : 이터러블을 값으로 받아(대개 배열) 그 안의 값을 복사해 셋에 넣어줌
  - `new Set()은 이터러블을 받지만, set.add는 이터러블이 아니더라도 괜찮음.(기능이 다르기때문)`
- set.add(value) : 값을 추가하고 셋 자신을 반환 //map도 map.set(key,value)는 맵 자신을 반환
  - set에 이미 있는 값을 add하면 아무런 반응이 없을 것임
- set.has(value) : 셋 내에 값이 존재하면 true,없으면 false 
- set.clear() :셋을 비웁니다.
- set.size: 셋에 몇 개의 값이 있는지 세어줌

### 예제
>방문자 방명록을 만든다고 가정해 봅시다. 한 방문자가 여러 번 방문해도 방문자를 중복해서 기록하지 않겠다고 결정 내린 상황입니다. 즉, 한 방문자는 '단 한 번만 기록’되어야 합니다.
```js
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// 어떤 고객(john, mary)은 여러 번 방문할 수 있습니다.
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// 셋에는 유일무이한 값만 저장됩니다.
alert( set.size ); // 3

for (let user of set) {
  alert(user.name); // // John, Pete, Mary 순으로 출력됩니다.
}
```

<br>

### Set의 값에 반복 작업하기 
- set.keys() : `set내의 모든 값을 포함하는 이터러블 객체를 반환`
  -  map.keys 는 요소들의 키값만을 모은 이터러블 객체를 반환
- set.values() : `set.keys()와 정확히 갖고, map과의 호환성을 보장하기위해 만들어짐`
  - map.values()는 각요소의 value 값만 모든 이터러블 객체를 반환
- set.entries(): 셋 내의 각 값을 이용해 만ㄷ는 `[value,value]` 배열을 포함하는 이터러블 객체를 반환
  - map과의 호환성을 위해 만들어짐 (map.entries()는 각 요소의 [key,value]를 모은 이터러블 객체 반환)


<br>

- __`여기에 더하여 맵은 배열과 유사하게 내장 메서드 forEach도 지원합니다.`__
```js
// 각 (키, 값) 쌍을 대상으로 함수를 실행
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 ...
});
```
<br>

```js
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// forEach를 사용해도 동일하게 동작합니다.
set.forEach((value, valueAgain, set) => {
  alert(value);
});

흥미로운 점이 눈에 띄네요. `forEach에 쓰인 콜백 함수는 세 개의 인수를 받는데)(value,value,set),` 첫 번째는 값, 두 번째도 같은 값인 valueAgain을 받고 있습니다. 세 번째는 목표하는 객체(셋)이고요. `동일한 값이 인수에 두 번 등장하고 있습니다.`

`이렇게 구현된 이유는 맵과의 호환성 때문입니다. ` 맵의 forEach에 쓰인 콜백이 `세 개의 인수(key,value,map)`를 받을 때를 위해서죠. 이상해 보이겠지만 이렇게 구현해 놓았기 때문에` 맵을 셋으로 혹은 셋을 맵으로 교체하기가 쉽습니다.`
```

<br>




- 출처: [모던 자바스크립트 튜토리얼 Map & Set](https://ko.javascript.info/map-set)
