```js
//가로 세로는 돌릴 수 있음 (자유롭게 전환 가능 = 가로 세로의 구분이 없다)
//즉 가로 세로중 큰값의 max, 작은값의 max를 곱해주면 최소한 모든 카드를 보장할 수 있음 
function solution(sizes) {
    var answer = 0;
    let maxWidth =[];
    let minWidth =[];
    for (let size of sizes){
        maxWidth.push(Math.max(size[0],size[1])); //가로-세로 두 값중에 큰 값
        minWidth.push(Math.min(size[0],size[1]));//가로-세로 두 값중에 작은 값 
    }
    answer = Math.max(...maxWidth)*Math.max(...minWidth);
    return answer;
}
```
