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

title: "[Programmers] 위장"
excerpt: "[Programmers] 위장"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, map, 해시]
date: 2021-07-19
last_modified_at: 2021-07-19
---
# [Programmers] 위장

Problem URL : [위장](https://programmers.co.kr/learn/courses/30/lessons/42578)

```cpp
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

int solution(vector<vector<string>> clothes) {
    unordered_map<string, int> um;
    for(int i = 0; i < clothes.size(); i++) {
        um[clothes[i][1]]++;
    }
    int answer = 1;
    for (auto iter = um.begin(); iter != um.end(); iter ++) {
        answer *= iter -> second + 1;
    }
    answer -= 1;
    return answer;
}
```
