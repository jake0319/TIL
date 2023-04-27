- 문제: https://school.programmers.co.kr/learn/courses/30/parts/12421
```js
function solution(begin, target, words) {
  const visited = { [begin] : 0 }; //시작단어값을 cp key로 hash.. (count값 참조 위해)
  const queue = [begin]; //queue에 시작노드(단어)넣기

  while(queue.length) { //큐가 존재하는한 반복
    const cur = queue.shift(); //현재 큐에서 shift한 단어
    
    if(cur === target) break; //현재뽑은 값이 target에 도달시 반복문탈출 
    
    for(const nxtWord of words) {
      if(isConnected(nxtWord, cur) && !visited[nxtWord]) { //1철자만다른단어(연결노드)이고, 방문하지않은 단어라면(큐에넣지않은 , visited에 key가 존재하지 않는경우)
        visited[nxtWord] = visited[cur] + 1; //방문처리시 단계마다 누적 count+1 //DEPTH를 세는 로직
        queue.push(nxtWord); //큐에 단어 넣기 
      }
    }
  }
  return visited[target] ? visited[target] : 0;
}

const isConnected = (str1, str2) => {
  let count = 0;
  const len = str1.length;
  //인수 두개 비교해서 
  for(let i = 0; i < len; i++) {
    if(str1[i] !== str2[i]) count++;
  }
  return count === 1 ? true : false;  //철자 1개만 다른경우만 true: 연결노드
}
```
