```js
function solution(n) {
    let pizzaCnt = 1;
    let answer; 
    while(pizzaCnt<=n){
        if(6*pizzaCnt%n ===0){  //6조각x피자판%n명 나머지 ===0으로 나누어떨어져야 m조각씩먹음
            answer=pizzaCnt;
            break; //정답이 나오면 반복문 탈출하고 return answer 
        }
        pizzaCnt++;// while반복문에선 반복조건++이 필수 
    }
    return answer;
}
```