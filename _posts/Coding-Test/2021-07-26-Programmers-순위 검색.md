---
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      comments: true
      share: true
      related: true

title: "[Programmers] 순위 검색"
excerpt: "[Programmers] 순위 검색"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 파라메트릭 서치, 문자열]
date: 2021-07-26
last_modified_at: 2021-07-26
---
# [Programmers] 순위 검색

Problem URL : [순위 검색](https://programmers.co.kr/learn/courses/30/lessons/72412?language=python3)

## 풀이1 (Python)

```python
from itertools import combinations as comb
from collections import defaultdict

def solution(infos, queries):
    answer = []
    info_dict = defaultdict(list)
    for info in infos:
        info = info.split()
        key_list = info[:-1] # key를 만들 재료들(점수 제외)
        score = int(info[-1])
        for i in range(5):
            for c in comb(key_list, i):
                key = ''.join(c) # key 생성
                # key 별 리스트에 append
                # key가 없어도 에러가 나지 않는 이유는 defaultdict이기 때문
                info_dict[key].append(score) 
                
    for key in info_dict.keys():
        info_dict[key].sort() # key 별 점수 리스트 정렬
        
    for query in queries:
        query = query.split(' ')
        score = int(query[-1])
        key_list = query[:-1] # query key를 만들 재료들(점수 제외)
        
        for i in range(3):
            key_list.remove('and') # key 재료들에서 and 제거
        while '-' in key_list:
            key_list.remove('-') # key 재료들에서 - 제거
        key = ''.join(key_list) # key 생성
        
        scores = info_dict[key]
        # scores가 비어있지 않으면 이분 탐색
        if len(scores) > 0:
            start, end = 0, len(scores) - 1
            lower_bound = -1 # 초기값이 -1임에 주의, 0보다도 아래여야 한다.
            while end >= start:
                mid = (start + end) // 2
                if scores[mid] >= score:
                    end = mid - 1
                else:
                    lower_bound = mid # score보다 낮은 점수일 때 lower_bound 갱신
                    start = mid + 1
            answer.append(len(scores) - 1 - lower_bound)
        
        # scores가 비어있으면
        else:
            answer.append(0)
            
    return answer
```

## 풀이 2(Python)

```python
from collections import defaultdict

def solution(infos, queries):
    answer = []
    info_dict = defaultdict(list)
    for info in infos:
        info = info.split()
        score = int(info[-1])
        key_element = [(info[0],'-'), (info[1],'-'), (info[2],'-'), (info[3],'-')] # key 재료들
        key = ''
        for i in range(2):
            for j in range(2): 
                for k in range(2):
                    for l in range(2):
                        key = key_element[0][i] + key_element[1][j] + key_element[2][k] + key_element[3][l]
                        info_dict[key].append(score)
                        key =''

    for key in info_dict.keys():
        info_dict[key].sort() # key 별 점수 정렬
        
    for query in queries:
        query = query.split(' ')
        score = int(query[-1])
        key_element = query[:-1] # query key를 만들 재료들(점수 제외)
        
        for i in range(3):
            key_element.remove('and') # key 재료들에서 and 제거
        key = ''.join(key_element) # key 생성
        
        scores = info_dict[key]
        # scores가 비어있지 않으면 이분 탐색
        if len(scores) > 0:
            start, end = 0, len(scores) - 1
            lower_bound = -1 # 초기값이 -1임에 주의, 0보다도 아래여야 한다.
            while end >= start:
                mid = (start + end) // 2
                if scores[mid] >= score:
                    end = mid - 1
                else:
                    lower_bound = mid # score보다 낮은 점수일 때 lower_bound 갱신
                    start = mid + 1
            answer.append(len(scores) - 1 - lower_bound)
        
        # scores가 비어있으면
        else:
            answer.append(0)
            
    return answer
```



## Comments

풀이 2는 풀이1에서 조금 수정했다.  
코드는 더 깔끔해졌지만, 효율은 오히려 떨어졌다...

문득 이진 탐색과 파라메트릭 서치의 차이점이 궁금해서 찾아보았다. [(링크1)](https://its2eg.tistory.com/entry/%EC%9D%B4%EC%A7%84%ED%83%90%EC%83%89Binary-Search%EC%99%80-%ED%8C%8C%EB%9D%BC%EB%A9%94%ED%8A%B8%EB%A6%AD-%EC%84%9C%EC%B9%98Parametric-Search) [(링크2)](https://nsgg.tistory.com/75) [(링크3)](https://heytech.tistory.com/m/97?category=453614)  
읽어보면 최적화 문제를 결정 문제로 바꿔서 푸는 것이 파라메트릭 서치라고 한다.  
종만북 450p를 시간날 때 읽어봐야겠다.