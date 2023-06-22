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

title: "[Programmers] 가장 먼 노드"
excerpt: "[Programmers] 가장 먼 노드"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 다익스트라, BFS]
date: 2021-08-10
last_modified_at: 2021-08-10
---
# [Programmers] 가장 먼 노드

Problem URL : [가장 먼 노드](https://programmers.co.kr/learn/courses/30/lessons/49189)

```cpp
#include <string>
#include <vector>
#include <queue>
#define INF 1e9

using namespace std;

int solution(int n, vector<vector<int>> edge) {
    int answer = 0;
    vector<vector<int>> adj;
    vector<int> dist;
    adj.resize(n + 1);
    dist.resize(n + 1);
    fill(dist.begin(), dist.end(), INF);
    int size = edge.size();
    for(int i = 0; i < size; i++) {
        adj[edge[i][0]].push_back(edge[i][1]);
        adj[edge[i][1]].push_back(edge[i][0]);
    }
    queue<int> q;
    q.push(1);
    dist[1] = 0;
    while(!q.empty()) {
        int now = q.front();
        q.pop();
        int size = adj[now].size();
        for(int i = 0; i < size; i++) {
            if(dist[adj[now][i]] > dist[now] + 1) {
                dist[adj[now][i]] = dist[now] + 1;
                q.push(adj[now][i]);
            }
        }
    }
    
    int maxDist = 0;
    for(int i = 2; i <= n; i++) {
        if(maxDist == dist[i]) {
            answer++;
        }
        if(maxDist < dist[i]) {
            maxDist = dist[i];
            answer = 1;
        }
    }
    
    return answer;
}
```