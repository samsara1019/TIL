# 전화번호 목록
javascript 사용이 안된다. 🙀 
## Q.
### 문제 설명
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.    
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.    
    
* 구조대 : 119
* 박준영 : 97 674 223
* 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

### 제한사항
* phone_book의 길이는 1 이상 1,000,000 이하입니다.
* 각 전화번호의 길이는 1 이상 20 이하입니다.

### 입출력 예
입출력 예는 링크를 통해서 확인해주세요.   
[직접 풀어보기](https://programmers.co.kr/learn/courses/30/lessons/42577)
    

## A.
### 나의 풀의
```java
import java.util.Arrays;
class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        Arrays.sort(phone_book);
        for(int i = 0; i < phone_book.length - 1; i++){
            if(phone_book[i + 1].startsWith(phone_book[i])){
                answer = false;
            }
        }
        return answer;
    }
}
```
배열을 정렬한 뒤, loop를 돌며 i + 1 번째의 요소가 i 번째의 요소로 시작하면 접두어인 경우로 판단하기로 했다.    

예를 들어서     
`[119, 97674223, 1195524421]`를 정렬하면    
`[119, 1195524421, 97674223]`가 되고, **1195524421**가 **119**로 시작되니까 한 번호가 다른 번호의 접두어인 경우이고, `false`를 반환한다.    

#### Java에서 Array 정렬하기
Java에서 배열을 정렬하는 방법을 검색해보니,`Arrays.sort()`함수를 사용하면 된다.    
**Arrays**를 사용하려면 `import java.util.Arrays;` 형식으로 패키지를 import 시켜줘야 한다.

#### Java에서 문자열이 특정 문자열로 시작되는지 알아내기
Java에도 Javascript와 같은 `startsWith`함수가 있었다.    

<hr />

출처: 프로그래머스 코딩 테스트 연습, [https://programmers.co.kr/learn/courses/30/lessons/42577](https://programmers.co.kr/learn/courses/30/lessons/42577)