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

title: "[Programmers] 리틀 프렌즈 사천성"
excerpt: "[Programmers] 리틀 프렌즈 사천성"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-08-12
last_modified_at: 2021-08-12
---
# [Programmers] 리틀 프렌즈 사천성

Problem URL : [리틀 프렌즈 사천성](https://programmers.co.kr/learn/courses/30/lessons/1836)

```cpp
#include <string>
#include <vector>
#include <map>
#define p pair<int, int>

using namespace std;

bool connectable(vector<string> &board, int x1, int y1, int x2, int y2) {
    if(x1 == x2) {
        if(y1 > y2) {
            swap(y1, y2);
        }
        for(int i = y1 + 1; i < y2; i++) {
            if(board[x1][i] != '.') {
                return false;
            }
        }
        return true;
    }else if(y1 == y2) {
        if(x1 > x2) {
            swap(x1, x2);
        }
        for(int i = x1 + 1; i < x2; i++) {
            if(board[i][y1] != '.') {
                return false;
            }
        }
        return true;
    }else {
        int dx = x2 > x1 ? 1 : -1;
        int dy = y2 > y1 ? 1 : -1;

        bool x = true;
        for(int i = x1 + dx; i != x2 + dx; i += dx) {
            if(board[i][y1] != '.'){
                x = false;
                break;
            }
        }
        if(x) {
            bool y = true;
            for(int i = y1 + dy; i != y2; i += dy) {
                if(board[x2][i] != '.'){
                    y = false;
                    break;
                }
            }
            if(y) {
                return true;
            }    
        }
        for(int i = y1 + dy; i != y2 + dy; i += dy) {
            if(board[x1][i] != '.'){
                return false;
            }
        }
        for(int i = x1 + dx; i != x2; i += dx) {
            if(board[i][y2] != '.'){
                return false;
            }
        }
        return true;
    }
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(int m, int n, vector<string> board) {
    string answer = "";
    map<char, vector<p>> M;
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(board[i][j] != '.' && board[i][j] != '*') {
                vector<p> tmp = M[board[i][j]];
                tmp.push_back({i,j});
                M[board[i][j]] = tmp;
            }
        }
    }
    int size = M.size();
    int cnt = 0;
    for(int i = 0; i < size; i++) {
        for(auto j : M) {
            if(connectable(board, j.second[0].first, j.second[0].second, j.second[1].first, j.second[1].second)) {
                answer += j.first;
                M.erase(j.first);
                board[j.second[0].first][j.second[0].second] = '.';
                board[j.second[1].first][j.second[1].second] = '.';
                cnt++;
                break;
            }
        }
    }
    if(cnt == size) {
        return answer;
    }else{
        return "IMPOSSIBLE";
    }
}
```

## Comments
connectable을 좀더 깔끔하게 짜고 싶다 ㅠ