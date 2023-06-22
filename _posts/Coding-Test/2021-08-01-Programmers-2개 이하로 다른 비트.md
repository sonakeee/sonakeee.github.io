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

title: "[Programmers] 2개 이하로 다른 비트"
excerpt: "[Programmers] 2개 이하로 다른 비트"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-08-01
last_modified_at: 2021-08-01
---
# [Programmers] 2개 이하로 다른 비트

Problem URL : [2개 이하로 다른 비트](https://programmers.co.kr/learn/courses/30/lessons/77885?language=cpp)

```cpp
#include <string>
#include <vector>

using namespace std;

vector<long long> solution(vector<long long> numbers) {
    vector<long long> answer;
    int size = numbers.size();
    for(long long &i : numbers) {
        if (i % 2 == 0){
            answer.push_back(i+1); // 짝수면 단순 + 1
        }else{
            long long x = 1;
            // 오른쪽 자리부터 처음 1이 아닌 숫자를 찾아준다.
            while( (x & i) != 0) {
                x = x << 1; 
            }
            answer.push_back(i + x / 2); // x만큼 더하고 x/2 만큼 빼주는 것
        }
    }
    return answer;
}
```
