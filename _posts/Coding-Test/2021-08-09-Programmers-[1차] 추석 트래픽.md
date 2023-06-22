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

title: "[Programmers] [1차] 추석 트래픽"
excerpt: "[Programmers] [1차] 추석 트래픽"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, 문자열]
date: 2021-08-09
last_modified_at: 2021-08-09
---
# [Programmers] [1차] 추석 트래픽

Problem URL : [[1차] 추석 트래픽](https://programmers.co.kr/learn/courses/30/lessons/17676)

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

int solution(vector<string> lines) {
    int answer = 0;
    vector<int> start, end;
    
    for(int i = 0; i < lines.size(); i++) {
        
        int h = stoi(lines[i].substr(11, 2)) * 3600 * 1000;
        int m = stoi(lines[i].substr(14, 2)) * 60 * 1000; 
        float s = stof(lines[i].substr(17, 6)) * 1000;        
        int process = stof(lines[i].substr(24, 5)) * 1000;
        start.push_back(h + m + s - process + 1); //시작 시간과 끝 시간 포함
        end.push_back(h + m + s);
    }
    
    for(int i = 0; i < lines.size(); i++) {
        // 정확히 완료시점부터 1초동안 계산하는 것이 각 로그별 최대 처리량이다.
        int end_time = end[i] + 1000;
        int count = 0;
        
        for(int j = i; j < lines.size(); j++)
        {
            if(start[j] < end_time)
                count++;
        }
        
        if(answer < count)
            answer = count;
    }
    return answer;
}
```

## Comments
```cpp
int s = stof(lines[i].substr(17, 6)) * 1000;  
```
이렇게 하면 틀린 경우가 생긴다. (ex lines[i].substr(17, 6) 가 4.002 일 때)  
원인은 stof가 오차가 있는 상태로 실수로 변환해주고, int는 소수점 자리를 버림하기 떄문이다.

```cpp
float x = stof("4.002");
float y = 4.002;
```
이 경우에는 x==y 가 true이고
```cpp
float x = stof("4.002") * 1000;
float y = 4.002 * 1000;
```
이 경우에는 false이다. 
출력하면 둘다 4002로 나오지만 int로 형변환하면 x는 4001, y는 4002가 나온다.