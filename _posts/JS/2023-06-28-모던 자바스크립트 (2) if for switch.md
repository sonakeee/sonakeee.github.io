---
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      comments: true
      related: true

title: "모던 자바스크립트 (2) if, for, switch"
excerpt: "모던 자바스크립트 (2)  if, for, switch"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - JS 
tags:
  - [JS, 모던자바스크립트]
date: 2023-06-28
last_modified_at: 2023-06-28
---
# if 문 
- if 문 대신 쓰던  condition ? value1 : value2 - 조건부연산자 삼항연산자 라고도 불림. (조건 연산자는 처음듣는 이름이라 적어봄..)
- 삼항연산자는 조건에 따라 반환 값을 달리하려는 목적으로 만들어짐. 가독성 때문에 사용 주의 
- 괄호안의 표현식을 불린값으로 변환함
- 0 , 빈 문자열 , null , undefined 는 기본적으로 false 로 변환됨 이외의 모든 값은 true

&nbsp; 

# 반복문
- 반복이 한번 진행되는것을 이터레이션이라고 함.
- break를 통해 반복문에서 빠져나오면 반복문 아래 첫 줄로 이동함 

```js
while(true) {
    break;
}
console.log(i)
```
- 여기선 반복문이 종료되면 console.log(i) 가 실행됨   

&nbsp; 

# whlie 문
- 흐름이 예정되지 않은 반복, 무한반복, 조건이 동적일 때 , 특정 조건에 만족해야할때까지 반복해야하면 사용
- condition이 truthy일때 반복문 코드가 실행
```js
while (condition) {
    console.log()
}

do {
    console.log()
} while (condition)
``` 
  
&nbsp;  

# do whlie
- 아래의 do while 문은 한번이라도 본문을 실행하고 싶을때 사용
  
&nbsp;  

# for 문
```js
for(let i = 0; i < 3; i++) {
    console.log(i)
}
```
- i = 0 , begin , 반복문에 진입할때 단 한 번 실행된다.
- i < 3 , condition , 반복마다 해당 조건이 확인됨, false일때 멈춤
- console.log(i) , body , condition이 truthy일동안 계속 실행
- i++ , step , 각 반복의 body가 실행된 이후에 실행됨.
- 처음 진입시 begin이 실행되고 이후 condition -> body -> step condition ... 으로 진행   
  

&nbsp; 


# continue 
```js
for (let i = 0; i < 10; i++) {
    if (i % 2 == 0) continue;
    console.log(i); // 1, 3, 5, 7, 9가 차례대로 출력됨
}

for (let i = 0; i < 10; i++) {
    if (i % 2) {
        alert( i );
    }
}
```
- 전체 반복문을 멈추지 않고 실행중인 이터레이션을 멈추고 다음 이터레이션을 강제로 실행시킴 
- 물론 if 문으로 대체가능하지만 전체 가독성이 떨어질 수 있음. 
- if안에 continue 는 가능하지만 삼항연산자로는 불가능하다 

&nbsp;


# labelName: for /break/continue 
```js
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    let input = prompt(`(${i},${j})의 값`, '');
    // 사용자가 아무것도 입력하지 않거나 Cancel 버튼을 누르면 두 반복문 모두를 빠져나옵니다.
    if (!input) break outer; // (*)
    // 입력받은 값을 가지고 무언가를 함
  }
}
alert('완료!');
```
- 레이블은 중첩 반복문을 한번에 빠져나갈 수 있는 유일한 방법 
- 이중 for문을 사용할 때 두개의 for문을 한번에 빠져나올때 쓰는 방법 
- break 대신 continue 를 사용할수도있다.

# switch
```js
let a = 3;

switch (a) {
  case 4:
    alert('계산이 맞습니다!');
    break;

  case 3: // (*) 두 case문을 묶음
  case 5:
    alert('계산이 틀립니다!');
    alert("수학 수업을 다시 들어보는걸 권유 드립니다.");
    break;

  default:
    alert('계산 결과가 이상하네요.');
}
```
- case문을 묶어서 사용할 수 있다.
- case의 자료형은 같아야 한다.
- 값을 비교할 때 === 를 사용한다. 고로 타입도 맞춰야한다.
- 

&nbsp;

  
문득 궁금해서 if와 switch문의 성능 차이에 대해서 검색해봤다.  
if 문은 조건문마다 cmp 명령문을 사용한다. (두 피연산자간의 조건 확인) 
switch문은 jump table을 사용하여 조건이 많아질수록 (if문의 조건이 3개에서 4개가 될때쯤..) 유리하다고 하는데 ...   
사실 성능을 따지기보단 가독성을 따져서 하는게 맞을정도의 성능차이라고 한다.  
  
어떨때 더 적합한 코드사용일까 생각 좀 해봐야겠다..    

