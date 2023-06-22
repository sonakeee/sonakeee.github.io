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

title: "[Programmers] 배달"
excerpt: "[Programmers] 배달"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 다익스트라]
date: 2021-07-16
last_modified_at: 2021-07-16
---
# [Programmers] 배달

Problem URL : [배달](https://programmers.co.kr/learn/courses/30/lessons/12978)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#define INF 1e9
#define p pair<int,int>

using namespace std;
int dist[51];
int route[51][51];
vector<vector<int>> branch;

int solution(int N, vector<vector<int> > road, int K) {
    int answer = 0;
    for(int i = 0; i <= 50; i++) {
        dist[i] = INF;
    }
    branch.resize(N+1); // [1]
    int size = road.size();
    for(int i = 0; i < size; i++) {
        int a = road[i][0];
        int b = road[i][1];
        int c = road[i][2];
        if(route[a][b] == 0) {
          route[a][b] = c;
          route[b][a] = c;
          branch[a].push_back(b);
          branch[b].push_back(a);
        }else {
          if(c < route[a][b]) {
            route[a][b] = c;
            route[b][a] = c;
          }
        }
    }

    priority_queue<p,vector<p>,greater<p>> pq;
    pq.push({1,0});
    dist[1] = 0;
    while(!pq.empty()) {
      int town = pq.top().first;
      int cost = pq.top().second;
      pq.pop();
      if(cost < dist[town]) {
        continue;
      }
      int num = branch[town].size();
      for(int i = 0; i < num; i++) {
        int nextTown = branch[town][i];
        int newCost = cost + route[town][nextTown];
        if(newCost < dist[nextTown]) {
          dist[nextTown] = newCost;
          pq.push({nextTown, newCost});
        }
      }
    }
    for(int i = 1; i <= N; i++) {
      if(dist[i] <= K) {
        answer ++;
      }
    }
    return answer;
}
```

## Comments  
[1] 자꾸 이 부분을 빼먹는다. 벡터 초기화에 신경쓰자