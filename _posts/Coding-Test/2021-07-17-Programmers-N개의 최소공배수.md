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

title: "[Programmers] N개의 최소공배수"
excerpt: "[Programmers] N개의 최소공배수"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 최소공배수, 최대공약수]
date: 2021-07-17
last_modified_at: 2021-07-17
---
# [Programmers] N개의 최소공배수

Problem URL : [N개의 최소공배수](https://programmers.co.kr/learn/courses/30/lessons/12953)

```cpp
#include <string>
#include <vector>

using namespace std;

int gcd(int a, int b){ 
    return (a % b == 0 ? b : gcd(b,a%b));
}

int solution(vector<int> arr) {
    int lcm = arr[0];
    int size = arr.size();
    for(int i = 1; i < size; i++) {
        int g;
        if(lcm > arr [i]) {
            g = gcd(lcm,arr[i]);
        }else {
            g = gcd(arr[i], lcm);
        }
        lcm = lcm * arr[i] / g;
    }
    return lcm;
}
```
