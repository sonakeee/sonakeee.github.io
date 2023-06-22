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

title: "[Programmers] 주식 가격"
excerpt: "[Programmers] 주식 가격"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, Stack]
date: 2021-04-30
last_modified_at: 2021-04-30
---
# [Programmers] 주식 가격

Problem URL : [주식 가격](https://programmers.co.kr/learn/courses/30/lessons/42584?language=cpp)

```cpp
#include <string>
#include <vector>
#include <stack>
#include <iostream>
#define p pair<int,int>
#define MAX_LENGTH 100000
using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer;
    stack<p> s;

    int size = (int)prices.size();
    int period[MAX_LENGTH] = { 0, };

    s.push({ prices[0],0 });
    for (int i = 1; i < size; i++) {
        while (!s.empty() && s.top().first > prices[i]) {           
            int idx = s.top().second;
            s.pop();
            period[idx] = i - idx;
        }
        s.push({ prices[i],i });
    }

    while (!s.empty()) {
        int idx = s.top().second;
        s.pop();
        period[idx] = (size - 1) - idx;
    }

    for (int i = 0; i < size; i++) {
        cout << period[i] << " ";
        answer.push_back(period[i]);
    }
    return answer;
}
```