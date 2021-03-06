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

- 다리를 지나는 다리 

>트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
> ※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

- 제한 사항

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.


```js
function solution(bridge_length, weight, truck_weights) {
    // answer: 걸린 시간
    let answer = 0;
    
    // queue: 현재 다리상태
    let queue = [];
    
    // queueSum: 현재 다리 무게
    let queueSum = 0;
    
    // queue의 길이는 다리 길이로 하고 다리 하나하나를 0으로 초기화
    for(let i =0;i<bridge_length;i++){
        queue.push(0);
    }
    
    // now_truck : 현재 다리를 지나가는 트럭
    let now_truck = truck_weights.shift();
    
    // 큐에 트럭을 넣고 다리를 앞으로 한칸씩 땡김
    queue.unshift(now_truck);
    queue.pop();
    
    // 다리 무게 증가
    queueSum += now_truck;
    
    // 시간 증가
    answer++;
    
    // 쉬지않고 큐에 트럭을 넣거나 다리를 건너기 때문에 queueSum 이 0이 되면 모든 트럭이 지나간 것.
    while(queueSum){ 
        // 다리에 있는 트럭 이동
        queueSum -= queue.pop();
        
        // 일단 다리를 안건넌 트럭 하나 빼고,
        now_truck = truck_weights.shift();
        
        // 다리에 들어갈 수 있으면 큐(다리)에 트럭 넣고 무게 증가
        if(now_truck+queueSum<=weight){
            queue.unshift(now_truck);
            queueSum+=now_truck;
        }
        // 다리에 들어갈 수 없으면 0을 넣고 뺏던거 다시 트럭집단에 고스란히 넣어줌
        else{
            queue.unshift(0);
            truck_weights.unshift(now_truck);
        }
        answer++;
    }
    return answer;
}

```

## 정렬

- 가장 큰수 

> 0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.
> 예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.
> 0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

- 제한 사항

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.


```js
function solution(numbers) {
    var answer = "";
    var tmp = numbers.sort(compare);

    for(let i = numbers.length-1; i>-1; i--){
       answer += tmp[i];
    }
  
    return answer.replace(/^0+/, "0");
}

function compare(a,b){
    if(b.toString()+a.toString()>a.toString()+b.toString())
        return -1;
}
```

- k번째수

> 배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.
> 예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면
> array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
> 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
> 2에서 나온 배열의 3번째 숫자는 5입니다.

- 제한 사항

- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

```js

function solution(array, commands) {
    var answer=[];
    for(let i = 0; i<commands.length; i++){
        let tmp = array.slice(commands[i][0]-1,commands[i][1]);
        tmp.sort(compare);
        answer.push(tmp[commands[i][2]-1])
        
    }
    return answer;
}

function compare(a, b) {
  if (a > b) return 1; // 첫 번째 값이 두 번째 값보다 큰 경우
  if (a == b) return 0; // 두 값이 같은 경우
  if (a < b) return -1; //  첫 번째 값이 두 번째 값보다 작은 경우
}

```
# Greedy

> 문제

## 문제 설명

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다.  

다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다.   

학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다.   

예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다.   

체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.  


전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.  


## 제한사항

전체 학생의 수는 2명 이상 30명 이하입니다.  

체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.  

여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.  

여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.  

여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다.   

<span class="highlight two-highlight" style ="background-color: #fff2ac;
    background-image: linear-gradient(to right, rgb(132, 250, 176) 0%, rgb(143, 211, 244) 100%);"> 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.</span>

## 입출력 예

|n    |lost  |reserve    |return    
|----|-------:|:-------|:-------:
|5|[2, 4]   |[1, 3, 5]|5|
|5|[2, 4] |[1, 3, 5]|4|
|3|[3]|[1, 3, 5]|2|


## 입출력 예 설명

예제 #1  
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.  

예제 #2  
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다  


## 풀이

- 어려운 문제가 아니다.

- 간단하게 도난당한 사람을 기준으로 여분이 있는 가장 가까운 번호부터 가져오면된다.

- 단) 도난당한 사람중에 여벌이있는 사람은 자신껄 입어야한다.

- 그렇기 때문에 처음 시작할때 도난당한 사람중에 여벌있는 사람은 제외하고 시작한다.



```js
function solution(n, lost, reserve) {
    var lost2=lost.filter(i => {if(!reserve.includes(i)) return true}) // 도난 당한사람중 여벌있는지 체크
    var reserve2=reserve.filter(l => {if(!lost.includes(l)) return true}) // 도난 당한사람중 여벌있는지 체크
    var count = test(n,lost2,reserve2)
  
    return n-lost2.length+count
}

let test = (n,lost,reserve)=>{
    var count = 0
    lost.filter(item => {
        const find_index =reserve.find(target => Math.abs(target-item) <=1)
        if(!find_index) return true
        reserve = reserve.filter(r => r !== find_index)
        count++
    })
    return count // lost.length를 해서 구현할수있지만 이편이 가독서잉 좋다고 판단
}
```
