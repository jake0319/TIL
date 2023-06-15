# 차집합은 Set객체에 filter와 set.has를조합하여 구한다.
```js
const set1 = new Set([1, 2, 3, 4, 5]);
const set2 = new Set([4, 5, 6, 7, 8]);

const difference = new Set([...set1].filter((x) => !set2.has(x)));
//set1의 요소중 set2에 없는것을 반환하는 이터러블로 새로운 차집합set을 만든다.

console.log([...difference]); // 출력: [1, 2, 3]
```
