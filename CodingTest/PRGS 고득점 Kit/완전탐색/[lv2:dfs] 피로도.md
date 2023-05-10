# 프로그래머스 고득점 kit - 완전탐색 - [lv2] 피로도

<br>

```js
function solution(k, dungeons) {
  let answer = 0;
  // 방문했는지 확인하기 위한 배열
  const visited = Array.from({ length: dungeons.length }, () => false); //던전size만큼 배열생성, false채우기
  //1. Array.from(iterable | arrayLike , mapFn )
  //2. mapFn은 iterable혹은 arrayLike의 원소를 순회하며 value로받는 콜백 
  // Array.from(iterable, (v,index)=>value+1); mapFn의 리턴값으로 새로운 배열이 생성된다.
    
  // 완전탐색을 위한 DFS(남은 피로도, 진행단계)
  function DFS(hp, L) {

    // 던전의 수 만큼 반복한다.
    for(let i=0; i<dungeons.length; i++){

      // 방문하지 않았고 현재 남은 피로도가 최소 필요도 보다 크거나 같으면 실행
        if(!visited[i] && dungeons[i][0] <= hp) {

        // 현재 들어온 던전을 방문 처리
        visited[i] = true;

        // DFS(현재 피로도 - 방문 던전, 진행단계 + 1)
        DFS(hp-dungeons[i][1],L+1);

        // DFS 종료 후 방문을 끝내준다.
        visited[i] = false;
      }
    }

    // 가장 깊이 들어간 진행단계를 answer에 넣어준다.
    answer = Math.max(answer,L); //answer을 max L값으로 계속 갱신      
  }
    DFS(k,0) //solution에서 dfs호출 
    return answer;   
}
```

<br>

### 참고
- [Array.from()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
- [프로그래머스 고득점 kit 완전탐색 - 피로도 솔루션](https://leejams.github.io/%ED%94%BC%EB%A1%9C%EB%8F%84/)
- https://school.programmers.co.kr/learn/courses/30/lessons/87946
