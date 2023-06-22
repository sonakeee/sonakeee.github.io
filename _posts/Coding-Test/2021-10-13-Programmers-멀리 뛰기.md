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

title: "[Programmers] 멀리 뛰기"
excerpt: "[Programmers] 멀리 뛰기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-10-13
last_modified_at: 2021-10-13
---
# [Programmers] 멀리 뛰기

Problem URL : [멀리 뛰기](https://programmers.co.kr/learn/courses/30/lessons/12914)

```cpp
#include <string>
#include <vector>
#include <cstring>

using namespace std;

long long solution(int n) {
    long long dp[n];
    memset(dp, 0, sizeof(dp));
    dp[0] = 1;
    dp[1] = 2;
    for(int i = 2; i < n; i++) {
        dp[i] = (dp[i-1] + dp[i-2]) % 1234567;
    }
    return dp[n-1];
}
```

## Comments
간단한 동적 프로그래밍 문제이다.