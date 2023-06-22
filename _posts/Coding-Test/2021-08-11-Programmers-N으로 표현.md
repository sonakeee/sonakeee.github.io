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

title: "[Programmers] N으로 표현"
excerpt: "[Programmers] N으로 표현"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-08-11
last_modified_at: 2021-08-11
---
# [Programmers] N으로 표현

Problem URL : [N으로 표현](https://programmers.co.kr/learn/courses/30/lessons/42895)

```cpp

#include <unordered_set>
#include <vector>

using namespace std;

int solution(int N, int number) {
    vector<unordered_set<int>> DP(9);
    int NN = N;
    for (int i = 1; i < 9; i++) {
        if (number == NN) {
            return i;
        }
        DP[i].insert(NN);
        NN = NN * 10 + N;
    }

    for (int k = 2; k < 9; k++) {
        for (int i = 1; i < k; i++) {
            for (int a : DP[i]) {
                for (int b : DP[k - i]) {
                    DP[k].insert(a + b);
                    if (a > b) {
                        DP[k].insert(a - b);
                    }
                    DP[k].insert(a * b);
                    if (a >= b) {
                        DP[k].insert(a / b);
                    }
                }
            }
        }
        if (DP[k].count(number)) {
            return k;
        }
    }

    return -1;
}
```