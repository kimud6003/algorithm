# Algorithm

본 문제들은 <a href= https://programmers.co.kr>프로그래머스</a>에서 푼 문제들을 적은 곳입니다.

## 해시 

- 1 완주하지 못한 선수

> 수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
> 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

- 제한 사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

- 풀이 : 선수의 수가 100,000이기에 O(n)으로 끝나야 할것을 예상했다. 따라서 정렬후 비교하여 해결하기로 하였다.

```js
function solution(participant, completion) {
    const total = participant.length;
    var answer = '';
    
    participant.sort();
    completion.sort();
    
    for(let i=0; i<total; i++){
        if(participant[i] !== completion[i]){
            answer = participant[i];
            return answer;
        }
    }
}
```

- 2 위장 

> 스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.
> 예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

- 제한 사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

- 풀이: 부위별 경우의 수는 의상의 갯수 +1 이라는 점을 생각했다.
- 예를 들어 머리에 (가발,모자,안경) 같은 물품으로 위장할수 있다면 가발 Or 모자 Or 안경 Or 아무것도 쓰지 않는다.

```js
function solution(clothes) {
    return Object.values(clothes.reduce((obj, t)=> {
        obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 1;
        return obj;
    } , {})).reduce((a,b)=> a*(b+1), 1)-1;    
}
```


## 스택

- 프린터 

> 일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

> 1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
> 2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
> 3. 그렇지 않으면 J를 인쇄합니다.

- 제한 사항

- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

- 풀이: 언급한 상황대로 구현  
```js
function solution(pri, loc) {
    let answer = 0;
    let list = pri.map((value,index)=>({
        v: value,
        i: index
    }));
    while(true){
        let tmp = list.shift();
        if(list.findIndex(a => a.v> tmp.v) ==-1){
           answer++; 
           if(tmp.i === loc){
                break;
            }
        }
        else{
            list.push(tmp);
        }
    }
    return answer;
}
```


