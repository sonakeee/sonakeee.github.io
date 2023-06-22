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

title: "[Programmers] 정수 삼각형"
excerpt: "[Programmers] 정수 삼각형"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-08-12
last_modified_at: 2021-08-12
---
# [Programmers] 정수 삼각형

Problem URL : [정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105)

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> triangle) {
    int answer = 0;
    int dp[500][500];
    dp[0][0] = triangle[0][0];
    int size = triangle.size();
    for(int i = 1; i < size; i++) {
        dp[i][0] = dp[i-1][0] + triangle[i][0];
        dp[i][i] = dp[i-1][i-1] + triangle[i][i];
        for(int j = 1; j < i; j++) {
            if(dp[i - 1][j - 1] > dp[i - 1][j]) {
                dp[i][j] = dp[i - 1][j - 1] + triangle[i][j];
            }else {
                dp[i][j] = dp[i - 1][j] + triangle[i][j];
            }
        }
    }
    for(int i = 0; i < size; i++) {
        if(answer < dp[size - 1][i]) {
            answer = dp[size - 1][i];
        }
    }
    
    return answer;
}
```

## Comments
간단한 DP 문제