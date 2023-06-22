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

title: "[Programmers] 체육복"
excerpt: "[Programmers] 체육복"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 그리디]
date: 2021-06-23
last_modified_at: 2021-06-23
---
# [Programmers] 체육복

Problem URL : [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

```cpp
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int solution(int n, vector<int> lost, vector<int> reserve) {
    int answer = 0;
    vector<int> v(n);
    fill(v.begin(), v.end(),1);
    for(int i : lost){
        v[i-1]--;
    }
    for(int i : reserve) {
        v[i-1]++;
    }
    if(v[0]==0 && v[1]==2){
        v[0] = 1;
        v[1] = 1;
    }
    
    for(int i = 1; i < n-1; i++) {
        if(v[i] == 0) {
            if(v[i-1] == 2){
                v[i-1] = 1;
                v[i] = 1;
                continue;
            }else if(v[i+1] == 2) {
                v[i] = 1;
                v[i+1] = 1;
            }
        }
    }
    
    if(v[n-1] == 0 && v[n-2] == 2){
        v[n-1] = 1;
        v[n-2] = 1;
    } 
    for(int i = 0; i < n; i++) {
        if(v[i] >= 1) {
            answer++;
        }
    }
    return answer;
}
```
