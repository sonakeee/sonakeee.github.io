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

title: "[BOJ] N-Queen"
excerpt: "[BOJ] N-Queen"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, DFS, 백트래킹]
date: 2021-10-02
last_modified_at: 2021-10-02
---
# [BOJ] N-Queen

Problem URL : [N-Queen](https://programmers.co.kr/learn/courses/30/lessons/12952?language=cpp)

```cpp
#include <string>
#include <vector>

using namespace std;

int answer;

bool check(int x, int y, vector<int> &col) {
    for(int i = 0; i < x; i++) {
        int rowDist = y - col[i];
        if(rowDist < 0) {
            rowDist *= -1;
        }
        if(y == col[i] || rowDist == x - i) {
            return false;
        }
    }
    return true;
}

void dfs(int x, int n, vector<int> &col) {
    if(x == n) {
        answer++;
        return;
    }
    
    for(int row = 0; row < n; row++) {
        if(check(x, row, col)) {
            col[x] = row;        
            dfs(x + 1, n, col);  // 매번 col[x] 값을 다르게 설정해주어서 탐색 [1]
        }
    }
}

int solution(int n) {
    answer = 0;
    vector<int> col(n);
    dfs(0, n, col);
    return answer;
}
```
## Comments
백트래킹으로 유명한 N-Queen 문제이다.  
[1]에서 어짜피 col[x] 값을 매번 새로 설정해주기 때문에   
값을 복구해주는 백트래킹 부분이 생략되었다.