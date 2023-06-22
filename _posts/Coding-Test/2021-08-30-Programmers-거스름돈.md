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

title: "[Programmers] 거스름돈"
excerpt: "[Programmers] 거스름돈"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-08-30
last_modified_at: 2021-08-30
---
# [Programmers] 거스름돈

Problem URL : [거스름돈](https://programmers.co.kr/learn/courses/30/lessons/12907)

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> money) {
    int dp[100001];
    dp[0] = 1;
    for(int i = 0; i < money.size(); i++) {
        for(int price = money[i]; price <= n; price++) {
            dp[price] += dp[price - money[i]];
        }
    }
    return dp[n];
}
```

## Comments
이전에 풀었던 [백준 - 동전1 문제](https://punsoo.github.io/coding%20test/BOJ-%EB%8F%99%EC%A0%841/) 와 똑같다!
