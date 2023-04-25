# 고득점킷 : DFS&BFS : [lv2] 타겟넘버
- 문제출처: https://school.programmers.co.kr/learn/courses/30/lessons/43165
```js
function solution(numbers, target) {
  let answer = 0;

  dfs(0, 0);
  return answer;

  function dfs(index, sum) {
    // 재귀 종료 (재귀적으로 호출되는 함수들의 종료조건)
    if (index === numbers.length) {
      if (sum === target) {
        answer++;
      }
      return;
    }

    dfs(index + 1, sum + numbers[index]); // depth(index) 와  +1로직 재귀호출
    dfs(index + 1, sum - numbers[index]); //  depth(index) 와 -1로직 재귀호출
  }
}

```
<br>
<br>

- 해답출처: https://codeisneverodd.com/solution-pass
