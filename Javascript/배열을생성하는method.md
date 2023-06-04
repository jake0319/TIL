# Array.from ( iterable , manFn , thisArg ) : 이터러블|arrayLike 로부터 매핑시킨 요소로 배열 생성
1. iterable : 이터러블(순회가능객체) 혹은 유사배열객체(length프로퍼티를 가진 객체) 
  - 유사배열객체 : length 프로퍼티 여부가 유사배열객체의 판단 요소이고,length프로퍼티가 있다면 해당 객체는 인덱싱이 가능하다(이터러블과 유사 취급)
2. mapFn: 이터러블의 요소를 순회하며 매핑시킬 함수 `ex: ()=>[]`
3. thisArg: mapFn에서 사용될 this객체
```js
Array.from( {length: 5} , ()=>[] ) 
// [ [],[],[],[],[] ]  빈배열5개를 갖는 이차원배열 생성 

<br>

# new Array(): 배열을 생성해준다.
- 생성할 배열 구성 요소를 직접 집어넣기 `ex: new Array(item1,item2,item3) => [item1,item2,item3]`
- 생성할 배열의 길이만 집어넣기: `ex: new Array(4) =>  [  ,  ,  ,  , ]  길이가 4인 배열 생성`

<br>

# new Array(n).fill(x) 
- n개의 length를 확보한 뒤 x요소로 배열을 채워넣습니다. (완전탐색의 방문배열 초기화 등에 사용)
- __`주의 : Array.fill메서드는 얕은복사를 하므로 모두 동일한 참조주소를 같습니다. 이차원배열에서는 사용시 반드시 주의를 해야합니다`.__

```js
let arr = new Array(4).fill([]);

for(let i=0;i<4;i++){
    for(let j=0;j<5;j++){ 
        arr[i][j]= i*j;
    }
}

console.log(arr);

예상값: 
0 0 0 0 0
0 1 2 3 4
0 2 4 6 8
0 3 6 9 12

실제값:
[0,3,6,9,12] // 1~5배열 모두 동일 참조값을 같는다.
[0,3,6,9,12] 
[0,3,6,9,12]
[0,3,6,9,12]
[0,3,6,9,12]
```
