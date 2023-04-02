```js
 function solution(slice, n) {
    //slice x R  = n x m(최소1조각: m=1로 가정후 문제풀기 : 최소조건)
    // R(1이상의 정수)= n/slice 가 정수 => n/slice ceiling(1이상)
    var answer = Math.ceil(n/slice);
    return answer;
}
```