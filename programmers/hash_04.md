# 베스트 앨범
어찌저찌 풀었는데.. 더 좋은 방법은 생각하지 못한 문제 😞    
다른분들의 풀이도 비슷 ~    

# Q.
## 문제 설명
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.    

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.
    
노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.    

## 제한사항
* genres[i]는 고유번호가 i인 노래의 장르입니다.
* plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
* genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
* 장르 종류는 100개 미만입니다.
* 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
* 모든 장르는 재생된 횟수가 다릅니다.

## 입출력 예
입출력 예는 링크를 통해서 확인해주세요.   
[직접 풀어보기](https://programmers.co.kr/learn/courses/30/lessons/42579)


# A.
## 나의 풀의
### 접근방법
1. 많이 재생된 '장르' 순서를 먼저 구하자.    
'장르' 별로 재생 수를 전부 합쳐서 재생 수를 기준으로 sort 하자.    
그럼 이런식으로 정렬되겠지?    
```js
[ { genre: 'pop', totalPlay: 3100 },
  { genre: 'classic', totalPlay: 1450 } ]
```

2. `genres` 배열과 `plays` 배열의 정보를 조합하여 새로운 배열을 만들자.
```js
[ { genre: 'classic', index: 0, plays: 500 },
  { genre: 'pop', index: 1, plays: 600 },
  { genre: 'classic', index: 2, plays: 150 },
  { genre: 'classic', index: 3, plays: 800 },
  { genre: 'pop', index: 4, plays: 2500 } ]
```

3. 이 배열을 장르별로 필터링하자.(`totalPlay` 수가 많은 장르부터)  
```js
[ { genre: 'pop', index: 4, plays: 2500 },
  { genre: 'pop', index: 1, plays: 600 } ]
```
```js
[ { genre: 'classic', index: 3, plays: 800 },
  { genre: 'classic', index: 0, plays: 500 },
  { genre: 'classic', index: 2, plays: 150 } ]
```
4. 장르별로 필터링 된 배열들을 다시 재생 수로 sort 하여 2개만 뽑자.


### 코드
```js
function solution(genres, plays) {
    const answer = [];
    const genreInfos = new Array();
    const MusicInfos = new Array()
    genres.forEach((genre, index) => {
        const musicInfo =
        {
            genre: genre,
            index: index,
            plays: plays[index],
        }
        MusicInfos.push(musicInfo)

        const isGenreExist = genreInfos.find(p => p.genre === genre)
        if (isGenreExist)
            isGenreExist.totalPlay = isGenreExist.totalPlay + plays[index]
        else
            genreInfos.push({ genre: genre, totalPlay: plays[index] })
    });


    const sortGerneByTotalPlay = genreInfos.sort((a, b) => {
        return b.totalPlay - a.totalPlay
    });
    sortGerneByTotalPlay.forEach(sortedGenreInfo => {
        const filteredByGenre = MusicInfos.filter(gp => gp.genre === sortedGenreInfo.genre)
        const sortByPlay = filteredByGenre.sort((a, b) => {
            return b.plays - a.plays
        })

        sortByPlay.forEach((sort, index) => {
            if (index > 1) return;
            answer.push(sort.index)
        })
    })
    return answer;
}
```

통과!  

<hr />

출처: 프로그래머스 코딩 테스트 연습, [https://programmers.co.kr/learn/courses/30/lessons/42579](https://programmers.co.kr/learn/courses/30/lessons/42579)