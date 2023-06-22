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

title: "[BOJ] 경주로 건설"
excerpt: "[BOJ] 경주로 건설"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, BFS]
date: 2021-09-28
last_modified_at: 2021-09-28
---
# [BOJ] 경주로 건설

Problem URL : [경주로 건설](https://programmers.co.kr/learn/courses/30/lessons/67259)

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct Road {
    int x, y, dir, cost;
    Road(int _x, int _y, int _dir, int _cost):x(_x), y(_y), dir(_dir), cost(_cost){};
};

int solution(vector<vector<int>> board) {
    int dx[4] = {1,0,-1,0};
    int dy[4] = {0,1,0,-1};
    int answer = 0;
    int r = board.size();
    int c = board[0].size();
    vector<vector<vector<int>>> dp(r, vector<vector<int>> (c, vector<int>(4, 1e9)));
    queue<Road> q;
    q.push(Road(0,0,0,0));
    q.push(Road(0,0,1,0));
    while(!q.empty()) {
        int x = q.front().x;
        int y = q.front().y;
        int dir = q.front().dir;
        int cost = q.front().cost;
        q.pop();
        if(cost > dp[x][y][dir]) {
            continue;
        }
        for(int d = 0; d < 4; d++) {
            int nx = x + dx[d];
            int ny = y + dy[d];
            if(nx < 0 || nx >= r || ny < 0 || ny >= c || board[nx][ny] == 1) continue;
            int ncost = cost + 100;
            if(d != dir) ncost += 500;
            if(dp[nx][ny][d] <= ncost) continue;
            dp[nx][ny][d] = ncost;
            q.push(Road(nx,ny,d,ncost));
        }
    }
    answer = dp[r-1][c-1][0];
    if(answer > dp[r-1][c-1][1]) {
        answer = dp[r-1][c-1][1];
    }
    return answer;
}
```
## Comments
익숙한 BFS 문제 유형, 금방 풀었다!(뿌듯)  
처음에는 움직인 거리가 (r-1+c-1)로 고정이라고 착각해서 꺾은 횟수만 카운트했다.  
예제를 보고 더 움직이는 경우가 있다는 것을 깨닫고 비용을 Road마다 저장해주었다!