```js
//일종의 구현문제와 비슷한 느낌임 

function solution(answers) {
  let stu_1 = [1, 2, 3, 4, 5];
  let stu_2 = [2, 1, 2, 3, 2, 4, 2, 5];
  let stu_3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];
  let col_1 = 0,col_2 = 0,col_3 = 0;

  for (let i = 0; i < answers.length; i++) {
    if (answers[i] === stu_1[i%5]) {      
      col_1++;     
    }
    if (answers[i] === stu_2[i%8]) {
      col_2++;
    }
    if (answers[i] === stu_3[i%10]) {
      col_3++;
    }
  }
  let score =[col_1,col_2,col_3];
  let result =[];
  //결국 가장 많이 맞춘 사람만 result에 들어가는데 동점인 경우 2명,3명도 가능  
  //즉 가장 큰 점수를 맞은 student(들)만 result에 push 
   
  //오름차순push를 굳이 sort로 구현할 필요는 없음! 
  for(let i =0; i<3; i++){
       //가장 큰점수와 일치 인덱스들이 오름차순으로 삽입
       if( score[i] === Math.max(...score)){
           result.push(i+1);
       }
  }
  return result
}
```
