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

title: "[Programmers] 방문 길이"
excerpt: "[Programmers] 방문 길이"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-07-16
last_modified_at: 2021-07-16
---
# [Programmers] 방문 길이

Problem URL : [방문 길이](https://programmers.co.kr/learn/courses/30/lessons/49994)

```cpp
#include <string>
#include <map>
using namespace std;

bool visit[11][11][11][11];
map<char, int> dx;
map<char, int> dy;

bool outRange(int x, int y) {
  if(x < 0 || x > 10 || y < 0 || y > 10) {
    return true;
  }
  return false;
}

int solution(string dirs) {
    int answer = 0;
    dx['U'] = 0;
    dy['U'] = 1;
    dx['D'] = 0;
    dy['D'] = -1;
    dx['L'] = -1;
    dy['L'] = 0;
    dx['R'] = 1;
    dy['R'] = 0;
    int x = 5, y = 5;
    int size = dirs.size();
    for(int i = 0; i < size; i++) {
      char dir = dirs[i];
      int nx = x + dx[dir];
      int ny = y + dy[dir];
      if(outRange(nx,ny)) {
        continue;
      }
      if(!visit[x][y][nx][ny]) {
        answer ++;
        visit[x][y][nx][ny] = true;
        visit[nx][ny][x][y] = true;
      }
      x = nx;
      y = ny;
    }

    return answer;
}
```
## Comments  
한번에 풀어서 기분이 좋다 ㅎㅎ