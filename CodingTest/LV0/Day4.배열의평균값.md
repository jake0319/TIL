```js
function solution(numbers) {
    let cnt =0;
    let sum =0;
    //cnt가 0부터 시작하면 n미만이어야함
    while(cnt<numbers.length){
        //외부스코프 sum을 변화시킴
        sum = sum + numbers[cnt];
        cnt = cnt+1;
    }
    let answer = sum /numbers.length;

    return answer;
}
```