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

title: "[Programmers] 다단계 칫솔 판매"
excerpt: "[Programmers] 다단계 칫솔 판매"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 트리, 구현]
date: 2021-08-18
last_modified_at: 2021-08-18
---
# [Programmers] 다단계 칫솔 판매

Problem URL : [다단계 칫솔 판매](https://programmers.co.kr/learn/courses/30/lessons/77486?language=cpp)

```cpp
#include <string>
#include <vector>
#include <map>

using namespace std;

vector<int> solution(vector<string> enroll, vector<string> referral, vector<string> seller, vector<int> amount) {
    vector<int> answer;
    vector<int> parent;
    map<string, int> m;
    int size = enroll.size();
    answer.resize(size);
    parent.resize(size);
    
    for(int i = 0; i < size; i++) {
      m.insert({enroll[i], i}); // enroll의 이름(값), 인덱스 순의 map을 하나 만들어주자!
    }
    
    for(int i = 0; i < size; i++) {
      if(referral[i] == "-") {
        parent[i] = -1; // 부모가 없는 노드
      }else {
        parent[i] = m[referral[i]];
      }
    }

    for(int i = 0; i < seller.size(); i++) {
      int profit = amount[i] * 100;
      int x = m[seller[i]];
      // 최상위 노드까지 반복
      while(x != -1) { 
        int dividend = int(profit * 0.1);
        answer[x] += profit - dividend;
        profit = dividend;
        x = parent[x];
      }
    }

    return answer;
}
```
