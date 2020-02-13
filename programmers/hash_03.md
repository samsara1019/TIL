# 위장

# Q.
## 문제 설명
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.    
    
예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.    
    
| 종류 | 이름 |
| --- | --- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠 |
| 하의 | 청바지 |
| 겉옷 | 긴 코트 |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.    

## 제한사항
* clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
* 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
* 같은 이름을 가진 의상은 존재하지 않습니다.
* clothes의 모든 원소는 문자열로 이루어져 있습니다.
* 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
* 스파이는 하루에 최소 한 개의 의상은 입습니다.

## 입출력 예
입출력 예는 링크를 통해서 확인해주세요.   
[직접 풀어보기](https://programmers.co.kr/learn/courses/30/lessons/42578)

# A.
## 나의 풀의
### 접근방법
처음에는 이렇게 접근했다.    
* 얼굴 3가지, 상의 2가지, 하의 2가지가 있다면?
1. 아이템 하나씩 착용했을 경우 3 + 2 + 2
2. 아이템을 전부 착용했을 경우 3 * 2 * 2
3. 1번과 2번을 더한다!

얼굴, 상의만 착용할 수도 있고 상의, 하의만 착용할 수도 있는데 그런 케이스는 생각을 못했다. 😢    
일부분만 착용하는 케이스는 어떻게 구하지..??    
    
그러던 중 구글링을 하여 힌트를 찾게 되었다.    
- 아이템마다 '착용하지 않는 경우'를 고려하여 1을 더해라
- '모두 착용하지 않는 경우'는 없으니 1을 빼라
즉 ((3+1) * (2+1) * (2+1)) - 1 이런식으로 구하면 된다.    


### 코드
```js
function solution(arr) {
    const clothesMap = new Map();
    arr.forEach(element => {
        clothesMap.set(element[1], clothesMap.get(element[1]) ? clothesMap.get(element[1]) + 1 : 2)
    });
    const countArr = Array.from(clothesMap.values())
    return countArr.reduce((a, b) => a * b) - 1
}
```
    
한 줄 한 줄 주석을 달아보자면
```js
function solution(arr) {
    // 종류별로 아이템 갯수를 셀 해시맵을 생성한다.
    const clothesMap = new Map();
    arr.forEach(element => {
        // 해시맵 key가 없다면 (최초) value에 2를 할당한다 ('안입는경우'를 미리 더함)
        // 해시맵 key가 있다면 기존 value에서 1을 더한다.
        clothesMap.set(element[1], clothesMap.get(element[1]) ? clothesMap.get(element[1]) + 1 : 2)
    });
    // .value()를 통해 해시맵의 value들만 object로 뽑아낸 뒤
    // Array.from으로 배열로 만든다 (reduce로 요소들을 모두 곱하기 위해)
    const countArr = Array.from(clothesMap.values())
    // reduce로 배열 요소를 모두 곱한 뒤 '아무것도 입지 않은 케이스'는 없으니 1을 빼준다
    return countArr.reduce((a, b) => a * b) - 1
}
```

### 멋진 풀의
```js
function solution(clothes) {
    return Object.values(clothes.reduce((obj, t)=> {
        obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 1;
        return obj;
    } , {})).reduce((a,b)=> a*(b+1), 1)-1;    
}
```
해시맵을 굳이 사용하지 않고 reduce로 배열의 요소를 바꿔버리는 방법!
    
<hr />

출처: 프로그래머스 코딩 테스트 연습, [https://programmers.co.kr/learn/courses/30/lessons/42578](https://programmers.co.kr/learn/courses/30/lessons/42578)