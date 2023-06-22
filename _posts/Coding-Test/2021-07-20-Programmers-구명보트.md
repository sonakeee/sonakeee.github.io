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

title: "[Programmers] 구명보트"
excerpt: "[Programmers] 구명보트"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 그리디]
date: 2021-07-20
last_modified_at: 2021-07-20
---
# [Programmers] 구명보트

Problem URL : [구명보트](https://programmers.co.kr/learn/courses/30/lessons/42578)

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> people, int limit) {
    int total = people.size();
    int answer = total;
    sort(people.begin(),people.end()); //[1]
    int idx = 0;
    int ridx = total - 1;
    int half = limit / 2;
    while(idx < total && ridx > idx && people[idx] <= half) {
        while(ridx > idx) {
            if(people[idx] + people[ridx] <= limit) {
                answer--;
                break;
            }
            ridx--;
        }
        idx++;
        ridx--;
    }
    return answer;
}
```

## Comments

[1] 두명이 타려면  작은 쪽이 절반이하의 몸무게여야한다.

작은 몸무게부터 탐색해서 남은 사람중 최대한 무거운 사람을 같이 태운다.(그리디)
