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

title: "[Programmers] 숫자 게임"
excerpt: "[Programmers] 숫자 게임"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 그리디]
date: 2021-10-03
last_modified_at: 2021-10-03
---
# [Programmers] 숫자 게임

Problem URL : [숫자 게임](https://programmers.co.kr/learn/courses/30/lessons/12987)

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    int b = B.size() - 1;
    for(int i = A.size() - 1; i >= 0; i--) {
        if(A[i] < B[b]) {  // 이길 수 있는 가장 큰 A의 점수랑 붙는다.
            answer++;
            b--;
        }
    }
    return answer;
}
```
## Comments
간단한 그리디 문제이다.  
B팀의 큰 점수부터 생각해서, 이길 수 있는 가장 큰 점수를 이기는 게 최선이다.