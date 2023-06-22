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

title: "[Programmers] 행렬 테두리 회전하기"
excerpt: "[Programmers] 행렬 테두리 회전하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 시뮬레이션]
date: 2021-06-24
last_modified_at: 2021-06-24
---
# [Programmers] 행렬 테두리 회전하기

Problem URL : [행렬 테두리 회전하기](https://programmers.co.kr/learn/courses/30/lessons/77485)

```cpp
#include <string>
#include <vector>

using namespace std;

int board[101][101];
int getMin(int a, int b) {
    if (a < b) {
        return a;
    }
    return b;
}
int rotate(int x1, int y1, int x2, int y2) {
    int min = 1e9;
    int tmp1 = board[x1][y2];   
    for (int i = y2; i > y1; i--) {
        board[x1][i] = board[x1][i-1];
        min = getMin(min, board[x1][i]);
    }
    int tmp2 = board[x2][y2];
    for (int i = x2; i > x1 + 1; i--) {
        board[i][y2] = board[i-1][y2];
        min = getMin(min, board[i][y2]);
    }
    board[x1 + 1][y2] = tmp1;
    min = getMin(min, board[x1+1][y2]);
    int tmp3 = board[x2][y1];
    for (int i = y1; i < y2 - 1; i++) {
        board[x2][i] = board[x2][i+1];
        min = getMin(min, board[x2][i]);
    }
    board[x2][y2 - 1] = tmp2;
    min = getMin(min, board[x2][y2 - 1]);
    for (int i = x1; i < x2 - 1; i++) {
        board[i][y1] = board[i + 1][y1];
        min = getMin(min, board[i][y1]);
    }
    board[x2 - 1][y1] = tmp3;
    min = getMin(min, board[x2 - 1][y1]);
    return min;
}

vector<int> solution(int rows, int columns, vector<vector<int>> queries) {
    vector<int> answer;
    
    for (int i = 1; i <= rows; i++) {
        for (int j = 1; j <= columns; j++) {
            board[i][j] = columns * (i - 1) + j;
        }
    }
    for (int i = 0; i < queries.size(); i++) {
        vector<int> v = queries[i];
        answer.push_back(rotate(v[0], v[1], v[2], v[3]));
    }
    return answer;
}
```
