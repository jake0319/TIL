- [프로그래머스 고득점 Kit _ 완전탐색 : 카펫 ](https://school.programmers.co.kr/learn/courses/30/lessons/42842)


<br>

```js
function solution(brown, yellow) {
    const sum = brown+yellow;
    let result =[];
    //문제조건상 가로는 항상 세로보다 크거나 같다,즉 높이는 제곱근이하 
    for(let h =3; h<=Math.sqrt(sum); h++){ 
        if (sum%h ===0)//나누어떨어지는 경우만 push 
        result.push([sum/h,h])
    }
 
    let answer =[];
    answer =result.filter((pair)=>{ //조건 필터링
        return (pair[0]-2)*(pair[1]-2) === yellow
    })
    
    return answer[0];
}
//1. brown + yellow = width x height
//2. w-2 x h-2 = yellow 
//3. b >= y , y>=3 
// 경계조건:  Math.sqrt(sum)   
// 완전탐색 : 모든케이스를 하나씩 넣어서 결과값 찾아내기 
```
1. brown,yellow는 주어지는 값 
2. 경계조건 찾고 ( 세로값은 제곱근 이하이다 , 가로 >= 세로 , 세로 >= 3)
3. height값으로 반복문 돌려서 `완전탐색하기` 
