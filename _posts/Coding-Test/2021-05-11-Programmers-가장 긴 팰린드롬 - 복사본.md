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

title: "[Programmers] 가장 긴 팰린드롬"
excerpt: "[Programmers] 가장 긴 팰린드롬"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 팰린드롬]
date: 2021-05-11
last_modified_at: 2021-05-11
---
# [Programmers] 가장 긴 팰린드롬

Problem URL : [가장 긴 팰린드롬](https://programmers.co.kr/learn/courses/30/lessons/12904?language=cpp)

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string>

using namespace std;

const int MAXN = 5000;
int A[MAXN];

string str;

void manachers(string S, int N){
    int r = 0, p = 0;
    for (int i = 0; i < N; i++){
        if (i <= r)
            A[i] = min(A[2 * p - i], r - i);
        else
            A[i] = 0;
        
        while (i - A[i] - 1 >= 0 && i + A[i] + 1 < N && S[i - A[i] - 1] == S[i + A[i] + 1])
            A[i]++;
        
        if (r < i + A[i]){
            r = i + A[i];
            p = i;
        }
    }
}

int solution(string s){
    int len = (int)s.size();
    
    for (int i = 0; i < len; i++)
    {
        str += '#';
        str += s[i];
    }
    str += '#';
    
    manachers(str, (int)str.size());
    
    len = (int)str.size();
    int ans = -1;
    for (int i = 0; i < len; i++)
        ans = max(ans, A[i]);
    
    return ans;
}
```

## Comments
유명한 알고리즘  
문자열 첫번째부터 탐색하면서, 탐색했던 팰린드롬 정보를 최대한 활용한다.  
2 * p - i 는 p - (i - p) 로 이해하면 된다.  
i에 관한 정보는 p를 기준으로 (i - p)만큼 이전 위치의 문자로부터 얻을 수 있기 떄문이다  
(p를 기준으로 팰린드롬이니까!)