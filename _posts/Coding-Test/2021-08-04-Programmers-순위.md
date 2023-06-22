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

title: "[Programmers] 순위"
excerpt: "[Programmers] 순위"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 그래프]
date: 2021-08-04
last_modified_at: 2021-08-04
---
# [Programmers] 순위

Problem URL : [순위](https://programmers.co.kr/learn/courses/30/lessons/49191)

```cpp
#include <string>
#include <vector>

using namespace std;

bool ch[101][101];

int solution(int n, vector<vector<int>> results) {
    int answer = 0;
    for(auto x : results) {
        ch[x[0]][x[1]] = true;
    }
    for(int k = 1; k <= n; k++){
        for(int i = 1; i <= n; i++){
            for(int j =1 ; j <= n; j++){
                if(ch[i][k] && ch[k][j]) { // i가 k를 이기고, k가 j를 이겼으면
                    ch[i][j] = true; // i가 j를 이긴다.
                }
            }
        }
    }
    for(int i = 1; i <= n; i++){
        int cnt = 0;
        for(int j = 1; j <= n; j++){
            if(ch[i][j] || ch[j][i]) {
                cnt++; //순위 비교가능
            }
        }
        if(cnt == n-1) {
            answer++; // n-1명과 순위를 비교 가능하면 순위를 매길 수 있다.
        }
    }
    return answer;
}
```
