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

title: "[Programmers] 게임 맵 최단거리"
excerpt: "[Programmers] 게임 맵 최단거리"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, BFS]
date: 2021-06-27
last_modified_at: 2021-06-27
---
# [Programmers] 게임 맵 최단거리

Problem URL : [게임 맵 최단거리](https://programmers.co.kr/learn/courses/30/lessons/1844)

```cpp
#include<vector>
#include<queue>
#include<cstring>
#define p pair<int,int>
#define MAX 1e9
using namespace std;


int solution(vector<vector<int> > maps)
{
    int dist[100][100];
    for(int i = 0; i < 100; i++){
        for(int j = 0; j < 100; j++){
            dist[i][j] = MAX;
        }
    }
    int dx[4] = { 1,-1,0,0 };
    int dy[4] = { 0,0,1,-1 };
    int row = maps.size() - 1;
    int col = maps[0].size() - 1;
    
    queue<p> q;
    q.push({ 0,0 });
    dist[0][0] = 1;
    while (!q.empty()) {
        int x = q.front().first;
        int y = q.front().second;
        q.pop();

        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            // dist 가 같거나 작으면 탐색할 필요가 없다.
            if ((nx < 0 || nx > row || ny < 0 || ny > col) || maps[nx][ny] == 0 || dist[nx][ny] <= dist[x][y] + 1 ) {
                continue;
            } else {
                dist[nx][ny] = dist[x][y] + 1;
                q.push({nx,ny});
            }
        }
    }
    if (dist[row][col] == MAX) {
        return -1;
    }
    return dist[row][col];
}
```
