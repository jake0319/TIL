- 문제: https://school.programmers.co.kr/learn/courses/30/lessons/1845
```js
// 최대한 다양한종류의 많은 포켓몬을 선택해야함 ,최대 N/2마리 
// 1. 중복제거 set size만큼 최대선택가능
// 2. Set.size가 N/2보다 크면 Set.size , 그게아니면 N/2 (nums.length/2)

function solution(nums) {
    var answer = 0;
    const log = new Set(nums);
    if(log.size >= nums.length/2) return nums.length/2
    return log.size;
}
```
- 팁) new Set(iterable)로 중복제거 이터러블을 생성하자 ! 
