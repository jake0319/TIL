# 완전탐색 - 소수판별 :dfs, 정수론(에라토스테네스의 체, 소수 필터링)
```js
function solution(numbers) { 
    return [...new Set(getPer(numbers))].filter(v => isPrime(v)).length; //소수판별 
}

const getPer = (str) => {
    const answer = [];
    const n = str.length;
    let ch = Array.from({ length: n }, () => 0);
    
    const dfs = (curStr) => {
        answer.push(+curStr);
        for (let i = 0; i < n; i++) {
            if (ch[i] === 0) {
                ch[i] = 1;
                dfs(curStr + str[i]); //str순회하며 조합 push (트리구조생각, 중복으로 넣어짐=>new Set()필요)
                ch[i] = 0;
            }
        }
    }
    dfs('');
    answer.shift();
    return answer;
}

const isPrime = (n) => { //에라토스테네스의 체 ( 2이상 srqt(n)이하에서 나눠지는 수가 있으면 소수아님 )
    if (n === 0 || n === 1) return false;
    for (let i = 2; i <= Math.sqrt(n); i++) {
        if (n % i === 0) {
            return false;
        }
    }
    return true;
}
```
1. dfs로 answer에 숫자조합 재귀호출로 넣기 (트리구조로 생각하며 넣고, 중복을 걸러야하므로 추후 set 고려)
2. 에라토스테네스의 체 ( isPrime() 구현))
3. dfs로 생성한 조합배열에 isPrime필터 적용 후 length 리턴 

### 솔루션
- https://school.programmers.co.kr/learn/courses/30/lessons/42839
- https://jsikim1.tistory.com/319
