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

title: "[Programmers] 멀쩡한 사각형"
excerpt: "[Programmers] 멀쩡한 사각형"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-06-27
last_modified_at: 2021-06-27
---
# [Programmers] 멀쩡한 사각형

Problem URL : [멀쩡한 사각형](https://programmers.co.kr/learn/courses/30/lessons/62048)

```cpp
using namespace std;

int gcd(int n,int m){
    if(n%m ==0){
        return m;
    }else {
        return gcd(m,n%m);
    }
}

long long solution(int w,int h) {
    long long answer = 1;
    int GCD = gcd(w,h);
    int W = w/GCD;
    int H = h/GCD;
    long long width = w;
    long long height = h;
    // 서로소인 n,m에 대하여 n X M 직사각형에서 대각선을 그었을 때
    // n + m - 1 개의 사각형과 교차한다.
    answer = width*height - (W+H-1) * GCD;
    return answer;
}
```
