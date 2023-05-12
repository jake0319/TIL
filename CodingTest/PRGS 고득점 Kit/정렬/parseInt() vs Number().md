
## String.parseInt()와 Number()의 차이 
- 문자열 사이에 숫자가들어간 경우 parseInt로 정수 변환불가 ex) '제1항' => NaN
- 하지만 어느 한쪽이 숫자인 경우 parseInt해석가능 ex) '2020년도' => 2020
- parseInt는 말그대로 문자열을 parse해서 정수를 뽑아내는 기능 ex) 23.123 => 23
- Number(23.123) => 23.123 그대로 모든 자릿수가 뽑아짐
