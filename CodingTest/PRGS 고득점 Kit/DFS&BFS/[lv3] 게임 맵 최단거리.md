- 문제 출처: https://school.programmers.co.kr/learn/courses/30/lessons/1844
```js

function solution(maps) {
    //BFS : 최단거리,간선비용1 
    const rows = maps.length-1, cols = maps[0].length-1;
    const queue =[[0,0,1]]; // 큐에 시작노드위치 x,y,거리(카운트할answer) 1 삽입 
    const DIRECTION = [[1, 0], [0, 1], [-1, 0], [0, -1]];
        //처음부터 도달할 수 없는 map이 주어진다면 -1 
        // if (maps[rows-1][cols] === 0 && maps[rows][cols-1] ===0){
        //  return -1
        // } 
        // else{ //도달불가한 map이 아닌경우
            while(queue.length){ //큐가비어있지 않다면 계속반복
            let [y,x,count] = queue.shift(); //2차원배열은 array[열][행] 임을 잊지말기 
                if(x===cols && y===rows) return count; //2차원그래프상 x =cols, y=rows
                for(let i=0; i<4; i++){
                let [dy,dx] = DIRECTION[i];
                let ny = y + dy; //렬
                let nx = x + dx; //행
                    //다음 이동위치가 도달할 수 없는 곳이라면 continue
                    if(nx< 0 || nx>cols || ny<0 || ny>rows) continue; 
                    //maps 내부이지만 벽이라면 continue
                    if(maps[ny][nx] === 0) continue;
                    //위 조건들을 통과한다면 해당지점을 방문처리 후 -> 해당지점을 Queue에 push
                    maps[ny][nx] =0; //방문처리(다음번 순회에서 도달할 수없도록 0으로 )
                    // count는 while문 스코프이므로 for문 내부에서 +1 로직이 동작하면 안됨(원본바꾸면안됨
                    // push되는 값에만 +1 시킬 수 있도록 (count+1 참조를 통해서만 push)
                    queue.push([ny,nx,count+1]) // 해당지점을 queue에 push
                }
                
            }
    //while문 내에서 답이 나온다면 queue가 비지 않으므로 
    return -1;
}
```

<br>

- [해설참조](https://codeisneverodd.com/solution-pass)
