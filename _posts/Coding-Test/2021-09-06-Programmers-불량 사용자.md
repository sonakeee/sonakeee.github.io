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

title: "[BOJ] 불량 사용자"
excerpt: "[BOJ] 불량 사용자"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, BFS, 문자열]
date: 2021-09-06
last_modified_at: 2021-09-06
---
# [BOJ] 불량 사용자

Problem URL : [불량 사용자](https://programmers.co.kr/learn/courses/30/lessons/64064)

## 첫 풀이

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;
bool dp[8][(1 << 8) + 1];
int answer;
int n, m;
vector<string> u;
vector<string> b;
void bfs(int ban_num, int visited) {
    dp[ban_num][visited] = true;
    if(ban_num == m) {
        answer++;
        return;
    }
    for(int i = 0; i < n; i++) {
        if(visited & (1 << i) || dp[ban_num + 1][visited | (1 << i)]) {
            continue;
        }
        if(u[i].length() != b[ban_num].length()) {
            continue;
        }
        bool match = true;
        int idx = u[i].length();
        while(--idx >= 0){
            if(b[ban_num][idx] != '*' && u[i][idx] != b[ban_num][idx]) {
                match = false;
                break;
            }
        }
        if(match) {
            bfs(ban_num + 1, visited | (1 << i));
        }
    }
}

int solution(vector<string> user_id, vector<string> banned_id) {
    answer = 0;
    u = user_id;
    b = banned_id;
    n = user_id.size();
    m = banned_id.size();
    bfs(0,0);
    return answer;
}
```

## 좀더 간단하게 풀은 풀이
```cpp
#include <string>
#include <vector>
#include <set>
#include <iostream>

using namespace std;
bool visit[(1 << 8) + 1];
int answer;
int n, m;
vector<string> u;
vector<string> b;
set<int> s;
void bfs(int ban_num, int visited) {
    if(ban_num == m) {
        s.emplace(visited);
        return;
    }
    for(int i = 0; i < n; i++) {
        if(visited & (1 << i)) {
            continue;
        }
        if(u[i].length() != b[ban_num].length()) {
            continue;
        }
        bool match = true;
        int idx = u[i].length();
        while(--idx >= 0){
            if(b[ban_num][idx] != '*' && u[i][idx] != b[ban_num][idx]) {
                match = false;
                break;
            }
        }
        if(match) {
            visit[visited | (1 << i)] = true;
            bfs(ban_num + 1, visited | (1 << i));
            visit[visited | (1 << i)] = false;
        }
    }
}

int solution(vector<string> user_id, vector<string> banned_id) {
    u = user_id;
    b = banned_id;
    n = user_id.size();
    m = banned_id.size();
    bfs(0,0);
    return s.size();
}
```