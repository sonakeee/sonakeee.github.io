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

title: "[BOJ] 합승 택시 요금"
excerpt: "[BOJ] 합승 택시 요금"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 플로이드-와샬]
date: 2021-09-25
last_modified_at: 2021-09-25
---
# [BOJ] 합승 택시 요금

Problem URL : [합승 택시 요금](https://programmers.co.kr/learn/courses/30/lessons/72413)

```cpp
#include <vector>
using namespace std;

int adj[201][201];

int solution(int n, int s, int a, int b, vector<vector<int>> fares) {
    int answer = 0;

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            adj[i][j] = 100000 * 200; //  [1]

    for (int i = 1; i <= n; i++) adj[i][i] = 0;

    for (auto fare : fares) {
        adj[fare[0]][fare[1]] = fare[2];
        adj[fare[1]][fare[0]] = fare[2];
    }

    for (int mid = 1; mid <= n; mid++)  // [2]
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                adj[i][j] = min(adj[i][j], adj[i][mid] + adj[mid][j]);

    answer = 1e9;

    for (int i = 1; i <= n; i++)
        answer = min(answer, adj[s][i] + adj[i][a] + adj[i][b]);

    return answer;
}
```
## Comments
[1] 충분히 큰 수로 초기화해줘야 한다.  
[2] mid가 가장 바깥 for문인 거 항상 기억하도록 하자!!