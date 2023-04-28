- 문제 : https://school.programmers.co.kr/learn/courses/30/lessons/43164
```js
function solution(tickets) {
    const answer = [];
    const goal = tickets.length + 1;
    const ch = Array.from({length: tickets.length}, _ => 0); //배열초기화(위 아래 같은 기능)
    // const ch = new Array(tickets.length).fill(0);
    
    const dfs = (path) => {
        if (path.length === goal) {
            answer.push(path)
        } else {
            for(const i in tickets) { //array의 index를 취할 때 사용 (obj의 key로도 사용)
                if(ch[i] === 0) {
                    const [start, end] = tickets[i]; //미방문 노드의 start
                    // console.log(tickets[i])
                    if (path[path.length - 1] === start) { //path의 마지막이 start와 같다면 
                        ch[i] = 1; //방문처리 
                        dfs([...path, end]); //dfs호출 
                        ch[i] = 0; //
                    }
                }
            }
        }
    }
    
    dfs(["ICN"]);
    //console.log(answer)
//    	[
//   [ 'ICN', 'SFO', 'ATL', 'ICN', 'ATL', 'SFO' ],
//   [ 'ICN', 'ATL', 'ICN', 'SFO', 'ATL', 'SFO' ],
//   [ 'ICN', 'ATL', 'SFO', 'ATL', 'ICN', 'SFO' ]
// ]
// 도달 가능한 route는 세가지인데 알파벳 순으로 정렬돼야 하므로 answer.sort()[0] 채택 
    return answer.sort()[0];
}
```
