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

title: "[BOJ] GPS"
excerpt: "[BOJ] GPS"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-09-07
last_modified_at: 2021-09-07
---
# [BOJ] GPS

Problem URL : [GPS](https://programmers.co.kr/learn/courses/30/lessons/1837)

```cpp
#include <algorithm>
#include <vector>

#define MAX 201
#define INF 1e9
using namespace std;

int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    vector<int> edge[MAX];
    vector<vector<int>> dp(k, vector<int>(n + 1, INF));
    for (int i = 0; i < m; i++) {
        int N1 = edge_list[i][0];
        int N2 = edge_list[i][1];
        edge[N1].push_back(N2);
        edge[N2].push_back(N1);
    }

    // dp[A][B] = C : A번째 경로가 B가 되었을 때, 수정해야 하는 최소 횟수
    // 시작할 때 출발점은 수정이 필요없고, 다른 거점은 불가능(INF)이다.
    dp[0][gps_log[0]] = 0;
    for (int i = 1; i < k; i++) {
        for (int Cur = 1; Cur <= n; Cur++) {
            if (dp[i - 1][Cur] == INF) continue;
            for (int j = 0; j < edge[Cur].size(); j++) {
                int next = edge[Cur][j];
                if (gps_log[i] != next) {
                    dp[i][next] = min(dp[i][next], dp[i - 1][Cur] + 1);
                } else {
                    dp[i][next] = min(dp[i][next], dp[i - 1][Cur]);
                }
            }
        }
    }

    if (dp[k - 1][gps_log[k - 1]] < INF) return dp[k - 1][gps_log[k - 1]];
    return -1;
}
```
## Comments
처음에 어떻게 풀어야할지 모르겠어서 풀이를 찾아보았다.  
dp를 이용한 것을 보고 아차 싶었다.  
[이 블로그](https://yabmoons.tistory.com/586) 풀이의 도움을 받았다.  