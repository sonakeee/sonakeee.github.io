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

title: "[Programmers] [1차] 프렌즈4블록"
excerpt: "[Programmers] [1차] 프렌즈4블록"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-08-02
last_modified_at: 2021-08-02
---
# [Programmers] [1차] 프렌즈4블록

Problem URL : [[1차] 프렌즈4블록](https://programmers.co.kr/learn/courses/30/lessons/17679?language=cpp)

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int M, N;
vector<vector<bool>> v;
int solution(int m, int n, vector<string> board);
bool check(int i, int j, vector<string> &board);
void erase(vector<string> &board);

int solution(int m, int n, vector<string> board) {
    M = m;
    N = n;
    int answer = 0;
    bool flag = false;
    while (!flag) {
        v = vector<vector<bool>>(m, vector<bool>(n, false)); // false로 초기화
        for (int i = 0; i < M - 1; i++) {
            for (int j = 0; j < N - 1; j++) {
                if (board[i][j] == 'x') 
                    continue;
                if (check(i, j, board))
                    flag = true;
            }
        }
        if (!flag) break;
        erase(board); // check 해놓은 블록들을 한번에 지운다.
        flag = false;
    }

    //answer계산
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'x')
                answer++;
        }
    }

    return answer;
}

//지울 블럭이 있으면 true, 없으면 false 반환
//지울 블럭들을 v에 체크한다.
bool check(int i, int j, vector<string> &board) {
    if(board[i][j] == board[i][j + 1] && board[i][j] == board[i + 1][j] && board[i][j] == board[i + 1][j + 1]) {
        v[i][j] = v[i+1][j] = v[i][j+1] = v[i+1][j+1] = 'x';
        return true;
    }
    return false;
}

void erase(vector<string> &board) {
    for(int a = 0; a < N; a++) {
        queue<int> q;
        for(int b = M - 1; b >= 0; b--) {
            if(!v[b][a]) {
                q.push(board[b][a]); // 지워진 블록을 제외하고 아래부터 순서대로 q에 넣는다.
            }
        }
        for(int b = M - 1; b >= 0; b--) {
            // 살아남은 블록들인 q로부터 board의 아래부터 다시 쌓아준다.
            if(q.empty()){
                board[b][a] = 'x';
            }else{
                board[b][a] = q.front();
                q.pop();
            }
        }
    }
}
```
