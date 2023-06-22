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

title: "[Programmers] 섬 연결하기"
excerpt: "[Programmers] 섬 연결하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, Prim]
date: 2021-05-14
last_modified_at: 2021-05-14
---
# [Programmers] 섬 연결하기

Problem URL : [섬 연결하기](https://programmers.co.kr/learn/courses/30/lessons/42861)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <functional>
#define p pair<int,int>

using namespace std;

bool visited[101];
vector<p> distances[101];

struct compare {
    bool operator()(p first, p second) {
        return first.second > second.second;
    }
};

int solution(int n, vector<vector<int>> costs) {
    for (int i = 0; i < costs.size(); i++) {
        int from = costs[i][0];
        int to = costs[i][1];
        int cost = costs[i][2];
        distances[from].push_back({ to, cost });
        distances[to].push_back({ from, cost });
    }

    int answer = 0;
    priority_queue<p,vector<p>,compare> pq;
    pq.push({ 0,0 });

    while (!pq.empty()) {
        int now = pq.top().first;
        int nowCost = pq.top().second;
        pq.pop();

        if (visited[now]) {
            continue;
        }

        visited[now] = true;
        answer += nowCost;

        for (int i = 0; i < distances[now].size(); i++) {
            int next = distances[now][i].first;
            int nextCost = distances[now][i].second;
            if (!visited[next]) {
                pq.push({ next,nextCost });
            }
        }
    }

    return answer;
}
```

## Comments  
프림(Prim) 알고리즘을 사용했다.
### 프림 알고리즘  
1. 특정 노드 하나를 임의로 고른다
2. 매단계 마다 새로운 노드를 추가해주는데, 갈 수 있는 노드 중에서 최소 비용인 노드를 고른다.
3. 모든 노드가 연결될 때가지 2를 반복