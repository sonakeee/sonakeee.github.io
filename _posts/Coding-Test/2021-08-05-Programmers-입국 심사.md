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

title: "[Programmers] 입국 심사"
excerpt: "[Programmers] 입국 심사"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 이분 탐색]
date: 2021-08-05
last_modified_at: 2021-08-05
---
# [Programmers] 입국 심사

Problem URL : [입국 심사](https://programmers.co.kr/learn/courses/30/lessons/43238)

```cpp
#include <vector>
#include <algorithm>

using namespace std;

long long solution(int n, vector<int> times) {
    long long answer = 0;
    int maxTime = 0;
    int timeSize = times.size();
    for(int i = 0; i < timeSize; i++) {
        if(maxTime < times[i]) {
            maxTime = times[i];
        }
    }

    long long start = 1;
    long long end = (long long) maxTime * n;
    long long mid;

    while(start <= end) {
        mid = (start + end) / 2;
        long long cnt = 0;

        for(int i = 0; i < times.size(); i++) {
            cnt += mid / times[i];
        }

        if(cnt < n) {
            start = mid + 1;
        }else {
            answer = mid;
            end = mid - 1;
        }
    }

    return answer;
}
```
