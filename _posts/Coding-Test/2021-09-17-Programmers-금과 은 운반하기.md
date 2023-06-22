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

title: "[BOJ] 금과 은 운반하기"
excerpt: "[BOJ] 금과 은 운반하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 파라메트릭 서치]
date: 2021-09-17
last_modified_at: 2021-09-17
---
# [BOJ] 금과 은 운반하기

Problem URL : [금과 은 운반하기](https://programmers.co.kr/learn/courses/30/lessons/86053)

```cpp
#include <string>
#include <vector>

using namespace std;
long long solution(int a, int b, vector<int> g, vector<int> s, vector<int> w, vector<int> t) {
    long long answer;

    long long start = 0;
    long long end = 10e14;

    int size = t.size();

    while (start <= end) {
        long long mid = (start + end) / 2;
        long long gold = 0;
        long long silver = 0;
        long long total = 0;

        for (int i = 0; i < size; i++) {
            long long move = mid / (t[i] * 2);
            if (mid % (t[i] * 2) >= t[i]) move++; // 마지막 편도는 돌아오지 않아도 된다.

            gold += (g[i] < move * w[i]) ? g[i] : move * w[i];
            silver += (s[i] < move * w[i]) ? s[i] : move * w[i];
            total += (g[i] + s[i] < move * w[i]) ? g[i] + s[i] : move * w[i];
        }

        if (gold >= a && silver >= b && total >= a + b) {
            end = mid - 1;
            answer = mid;  // 조건을 만족했을 때 정답 후보 [1]
        } else {
            start = mid + 1;
        }
    }

    return answer;
}
```

## Comments
gold >= a && sil >= b && total >= a + b  
이 조건을 생각해내는 것이 중요하다.

그리고 [1]의 answer = mid; 의 경우, 항상 더 작은 값을 탐색하게 된다. ( min을 해줄 필요가 없다.)