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

title: "[Programmers] 행렬의 곱셈"
excerpt: "[Programmers] 행렬의 곱셈"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-07-18
last_modified_at: 2021-07-18
---
# [Programmers] 행렬의 곱셈

Problem URL : [행렬의 곱셈](https://programmers.co.kr/learn/courses/30/lessons/12949)

```cpp
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> solution(vector<vector<int>> arr1, vector<vector<int>> arr2) {
    vector<vector<int>> answer;
    int r = arr1.size();
    int c = arr1[0].size();
    int c2 = arr2[0].size();
    answer.resize(r);
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c2; j++){
            int sum = 0;
            for (int k = 0; k < c; k++) {
                sum += arr1[i][k] * arr2[k][j];
            }
            answer[i].push_back(sum);
        }
    }
    return answer;
}
```
