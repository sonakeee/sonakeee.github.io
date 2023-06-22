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

title: "[BOJ] 등굣길"
excerpt: "[BOJ] 등굣길"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 동적 프로그래밍, 구현]
date: 2021-09-10
last_modified_at: 2021-09-10
---
# [BOJ] 등굣길

Problem URL : [등굣길](https://programmers.co.kr/learn/courses/30/lessons/42898)

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int solution(int m, int n, vector<vector<int>> puddles) {
    vector<vector<int>> board(n+1, vector<int>(m+1, 1));
    for(int i = 0; i < n + 1; i++) {
        board[i][0] = 0;
    }
    for(int i = 0; i < m + 1; i++) {
        board[0][i] = 0;
    }
    for(int i = 0; i < puddles.size(); i++) {
        int r = puddles[i][1];
        int c = puddles[i][0];

        board[r][c] = 0;
    }

    vector<vector<int>> way(n+1, vector<int>(m+1, 0));
    way[1][1] = 1;
    for(int r = 1; r <= n; r++) {
        for(int c = 1; c <= m; c++) {
            if(board[r][c] == 0)
                continue;
            
            way[r][c] = (way[r-1][c] + way[r][c-1] + way[r][c]) % 1000000007;
        }
    }

    return way[n][m];
}
```

## Comments
간단한 DP 문제이다.