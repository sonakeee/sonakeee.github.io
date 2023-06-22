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

title: "[Programmers] 보석 쇼핑"
excerpt: "[Programmers] 보석 쇼핑"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 투포인터]
date: 2021-08-28
last_modified_at: 2021-08-28
---
# [Programmers] 보석 쇼핑

Problem URL : [보석 쇼핑](https://programmers.co.kr/learn/courses/30/lessons/67258)

```cpp
#include <string>
#include <vector>
#include <map>
#include <iostream>

using namespace std;

vector<int> solution(vector<string> gems) {
    vector<int> answer;
    map<string, int> m;
    string tmp;
    int size = gems.size();
    for(int i = 0; i < size; i++) {
        m[gems[i]]++;
    }
    int full = m.size();
    int start = 0;
    int end = 0;
    m.clear();
    m[gems[0]]++;
    int x = 0;
    int y = size;
    while(start <= end) {
        if(m.size() == full) {
            if(y - x > end - start) {
                y = end;
                x = start;
            }
            if(--m[gems[start]] == 0) {
                m.erase(gems[start]);  // [1]
            }
            start++;
        }else{            
            end++;
            if(end == gems.size()) {
                break;
            }
            m[gems[end]]++;
        }
    }
    answer.push_back(x + 1);
    answer.push_back(y + 1);
    return answer;
}
```
## Comments
간단한 투포인터 문제이다.  
[1] 0 값으로 만들어주어도 erase를 따로해주어야 size에서 제외된다.