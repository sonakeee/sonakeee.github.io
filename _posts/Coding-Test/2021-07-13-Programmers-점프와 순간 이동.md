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

title: "[Programmers] 점프와 순간 이동"
excerpt: "[Programmers] 점프와 순간 이동"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-07-13
last_modified_at: 2021-07-13
---
# [Programmers] 점프와 순간 이동

Problem URL : [점프와 순간 이동](https://programmers.co.kr/learn/courses/30/lessons/12980)

```cpp
#include <iostream>
using namespace std;

int solution(int n)
{
    int ans = 0;
    
    // 역으로 처음 위치로 돌아간다.
    while(n > 1) {
        if(n % 2 == 1) {
            ans ++; // n이 홀수이면 1칸 이동
            n -= 1;
        } else {
            n /= 2;
        }
    }

    return ans + 1 // 마지막에 1에서 0으로 한번 더 이동;
}
```