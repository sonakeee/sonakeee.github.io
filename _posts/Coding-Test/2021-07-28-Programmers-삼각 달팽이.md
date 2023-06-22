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

title: "[Programmers] 삼각 달팽이"
excerpt: "[Programmers] 삼각 달팽이"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, DFS, 구현]
date: 2021-07-28
last_modified_at: 2021-07-28
---
# [Programmers] 삼각 달팽이

Problem URL : [삼각 달팽이](https://programmers.co.kr/learn/courses/30/lessons/68645?language=cpp)

```cpp
#include <string>
#include <vector>
#include <cstring>
using namespace std;

int map[1000][1000];
int dx[3] = {1,0,-1};
int dy[3] = {0,1,-1};
int N;

bool movable(int x, int y, int dir) {
    x = x + dx[dir];
    y = y + dy[dir];
    if(x < 0 || x >= N || y < 0 || y >= N || map[x][y] != 0) {
        return false;
    }
    return true; 
}

void dfs(int x, int y, int count, int dir) {
    if(!movable(x,y,dir)){
        return; // 첫 움직임부터 못움직이면 끝
    }
    while(movable(x,y,dir)) {      
        x = x + dx[dir];
        y = y + dy[dir];
        map[x][y] = ++count;  
    }
    dir = (dir + 1) % 3; // 반시계 방향으로 회전
    dfs(x,y,count,dir);
}

vector<int> solution(int n) {
    vector<int> answer;
    memset(map,0,sizeof(map));
    N = n;
    map[0][0] = 1;
    dfs(0,0,1,0);
    for(int i = 0; i < N; i++) {
        for(int j = 0; j <= i; j++) {
             answer.push_back(map[i][j]);
        }
    }
    return answer;
}
```
