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

title: "[BOJ] 이중우선순위큐"
excerpt: "[BOJ] 이중우선순위큐"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 우선순위 큐, 힙]
date: 2021-09-09
last_modified_at: 2021-09-09
---
# [BOJ] 이중우선순위큐

Problem URL : [이중우선순위큐](hhttps://programmers.co.kr/learn/courses/30/lessons/42628)

## 힙(우선순위 큐)을 이용한 풀이

```cpp
#include <iostream>
#include <queue>
#include <sstream>
#include <string>
#include <vector>
using namespace std;

vector<int> solution(vector<string> operations) {
    vector<int> answer;
    int cnt = 0;

    priority_queue<int> maxHeap;                             // 내림차순 (디폴트)
    priority_queue<int, vector<int>, greater<int>> minHeap;  // 오름차순
    char c;
    int n;
    for (int i = 0; i < operations.size(); i++) {
        c = operations[i][0];
        n = stoi(operations[i].substr(2));
        if (cnt == 0) {
            while (!maxHeap.empty()) maxHeap.pop();
            while (!minHeap.empty()) minHeap.pop();
        }
        if (c == 'I') {
            maxHeap.push(n);
            minHeap.push(n);
            cnt++;
        } else {
            if (cnt != 0) {
                if(n == 1) {
                    maxHeap.pop();
                }else{
                    minHeap.pop();
                }
                cnt--;
            }
        }
    }
    if (cnt == 0) {
        answer.push_back(0);
        answer.push_back(0);
    } else {
        answer.push_back(maxHeap.top());
        answer.push_back(minHeap.top());
    }
    return answer;
}
```
## STL multiset을 이용한 풀이
```cpp
#include <iostream>
#include <set>
#include <sstream>
#include <string>
#include <vector>
using namespace std;

vector<int> solution(vector<string> operations) {
    vector<int> answer;
    int cnt = 0;
    multiset<int> ms;  // 오름차순(디폴트)
    char c;
    int n;
    for (int i = 0; i < operations.size(); i++) {
        c = operations[i][0];
        n = stoi(operations[i].substr(2));
        if (c == 'I')
            ms.insert(n);
        else {
            if(!ms.empty()) {
                if(n == 1){
                    ms.erase(--ms.end());
                }else {
                    ms.erase(ms.begin());
                }
            }
        }
    }
    if (ms.empty()) {
        answer.push_back(0);
        answer.push_back(0);
    } else {
        answer.push_back(*(--ms.end()));
        answer.push_back(*ms.begin());
    }
    return answer;
}
```

## Comments
풀이를 찾다가 multiset이라는 강력한 자료구조를 알게 되었다.  
자동 정렬이 되고, 중복된 키 값을 가질 수 있는 자료구조이다.