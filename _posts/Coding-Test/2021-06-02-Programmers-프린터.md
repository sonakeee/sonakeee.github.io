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

title: "[Programmers] 프린터"
excerpt: "[Programmers] 프린터"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, queue, 우선순위 큐]
date: 2021-06-02
last_modified_at: 2021-06-02
---
# [Programmers] 프린터

Problem URL : [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)

```cpp
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> priorities, int location) {
    int answer = 0;
    priority_queue<int> pq;
    queue<pair<int, int>> q;

    int size = priorities.size();
    for (int i = 0; i < size; i++) {
        q.push(make_pair(i, priorities[i]));
        pq.push(priorities[i]);
    }

    while (!q.empty()) {
        int index = q.front().first;
        int value = q.front().second;
        q.pop();

        if (value == pq.top()) {
            // 제일 큰 값이면 프린트한다.
            pq.pop();
            answer++;
            if (index == location) {
                break;
            }
        } else {
            q.push({ index, value });
        }
    }

    return answer;
}
```
