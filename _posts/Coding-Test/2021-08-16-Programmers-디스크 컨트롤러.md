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

title: "[Programmers] 디스크 컨트롤러"
excerpt: "[Programmers] 디스크 컨트롤러"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 힙, 우선순위 큐]
date: 2021-08-16
last_modified_at: 2021-08-16
---
# [Programmers] 디스크 컨트롤러

Problem URL : [디스크 컨트롤러](https://programmers.co.kr/learn/courses/30/lessons/42627#)

```cpp
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

struct cmp {
    bool operator()(pair<int, int>&a, pair<int, int>&b) {
        if (a.second == b.second) {
            return a.first > b.first;
        }else {
            return a.second > b.second;
        }
    }
};

int solution(vector<vector<int>> jobs) {
    int answer = 0;
    priority_queue<pair<int,int>,vector<pair<int,int>>, cmp> pq;
    // 작업이 요청되는 시점이 작은 순으로, 같으면 작업의 소요시간이 작은 순으로 정렬
    sort(jobs.begin(),jobs.end());
    int t = jobs[0][0];
    int sum = 0;
    pq.push({jobs[0][0], jobs[0][1]});
    int idx = 1;
    int size = jobs.size();
    int popNum = 0;
    while(popNum != size) {
      if(pq.empty()) {
        // 작업할게 없을 때, 요청되는 시점이 제일 빠르고, 소요시간이 제일 적은 작업을 큐에 넣는다.
        pq.push({jobs[idx][0],jobs[idx][1]});
        t = jobs[idx][0];
      }else {
        t += pq.top().second;
        sum += t - pq.top().first;
        pq.pop();
        popNum++;
        while(idx < size && jobs[idx][0] <= t) {
          pq.push({jobs[idx][0], jobs[idx][1]});
          idx++;
        }
      }
    }

    answer = sum / size;
    return answer;
}
```
