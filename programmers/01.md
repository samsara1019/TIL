
# 완주하지 못한 선수
## Q.
### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.   
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
### 제한사항
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.   
completion의 길이는 participant의 길이보다 1 작습니다.   
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.   
참가자 중에는 동명이인이 있을 수 있습니다.
### 입출력 예
입출력 예는 링크를 통해서 확인해주세요.   
[직접 풀어보기](https://programmers.co.kr/learn/courses/30/lessons/42576)
    

## A.
### 나의 풀의
```javascript
function solution(participant, completion) {
    participant.sort();
    completion.sort();
    for (let i = 0; i < participant.length; i++) {
        if (participant[i] !== completion[i]) return participant[i];
    }
}
```
이중 for문을 피하기 위해 Array의 기본 함수 `sort()`를 이용하여 먼저 정렬을 했습니다.   
`sort()`함수는 compareFunction이 제공되지 않으면, 배열의 요소를 모두 문자열로 변환한 뒤 유니 코드 순서로 정렬합니다.   
두 배열을 정렬한 뒤, 두 배열을 바교해서 같은 인덱스에 다른 요소가 있다면 그 요소가 바로 완주하지 못한 사람의 이름이겠죠!   
    
#### `for in`문을 사용한다면 더 깔끔할텐데...🤔
`for in`문은 객체의 모든 프로퍼티를 열거합니다.   
배열도 객체이기 때문에, `for in`문은 불필요한 프로퍼티가 출력될 수 있습니다.   
물론, 이 예제에서는 그렇지 않지만 배열에서 `for in`문 사용을 지양하는 편입니다.
### 멋진 풀의
```javascript
const solution = (participant, completion) => {
    participant.sort()
    completion.sort()
    while (participant.length) {
        let result = participant.pop()
        if (result !== completion.pop()) return result
    }
}
```
`while`문을 돌면서 `pop()`하는 방법도 있네요!
<hr />

출처: 프로그래머스 코딩 테스트 연습, [https://programmers.co.kr/learn/courses/30/lessons/42576](https://programmers.co.kr/learn/courses/30/lessons/42576)