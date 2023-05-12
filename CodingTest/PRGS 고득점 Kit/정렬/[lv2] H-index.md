# [정렬 : lv2]  H-index 
```js
//논문이 h를 기준으로 나뉘어야함 
function solution(citations) {
   let length=0;
   let h = 0;
   while(length>=h){
   h++    
   length = citations.filter(citation => citation>=h).length
   }
   return h-1
}
```
1. filter는 자동으로 h이상인 논문을 걸러줌.
2. h의 최댓값을 구하라고 했으므로 h가 최대가 되는 경계조건을 생각해보자
3. h는 0에서 점점 커지며 length는 반대로 줄어들 것임
4. length < h 로 전환되는 지점의 h가 가장큰 h! 

- 문제: https://school.programmers.co.kr/learn/courses/30/lessons/42747
