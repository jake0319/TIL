# [lv3]네트워크 
## 재귀호출하지 않는 DFS 
- 문제출처 : https://school.programmers.co.kr/learn/courses/30/lessons/43162

```js
// 연결요소 
// 컴퓨터n개(: 0 ~ n-1 노드)
// 연결요소 판단 2차원배열 computers[i][j] =1 ,아니면 0
// n번컴퓨터에서 연결되는 요소들을 이어가며 확인하고, 연결요소 개수를 카운트
// DFS이지만 재귀호출로 푸는 문제가 아님

// 시작컴퓨터~연결~연결끝 (1네트워크) | 단일 네트워크  
function solution(n,computers){
    let answer = 0;
    //연결요소 카운트
    const visited = new Array(n).fill(false);
    //연결요소들끼리의 임시배열
    //현재컴퓨터와 다음컴퓨터(현재+1)이 미연결이면 연결요소가 종료, 다음컴퓨터 방문처리불가
    const countNetwork = (startNode)=>{
        //각 컴퓨터마다 네트워크 탐색 (반복호출))
        const connectedItemsArray = [startNode]; //시작점부터 네트워크여부 탐색
            while(connectedItemsArray.length>0){
            const currentNode =connectedItemsArray.pop();  //시작노드가 현재노드로
                visited[currentNode]=true;// 방문처리
                for(let nextNode =0; nextNode<n; nextNode++){
                    //다음노드가 미방문이고 현재노드와 연결시 연결노드배열에 push 
                if(!visited[nextNode] && computers[currentNode][nextNode])
                    {
                        //가장마지막 연결된 요소가 push됨,그리고 pop후 다음 startNode가됨
                    connectedItemsArray.push(nextNode)
                    }
                }
        }
    }
    for (let startNode=0; startNode<n; startNode++){
            //연결되어 이미 방문한 네트워크는 pass된다 
        if(!visited[startNode]){//미방문이면 해당노드에서 네트워크찾기실행
            countNetwork(startNode) //시작노드가 몇번 들어가는지?
            answer++; //countNetwork함수 실행마다 
        }
    }
    return answer
}

```

- 해설출처: https://codeisneverodd.com/solution-pass
