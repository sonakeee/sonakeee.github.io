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

title: "[Programmers] 여행경로"
excerpt: "[Programmers] 여행경로"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, DFS]
date: 2021-10-04
last_modified_at: 2021-10-04
---
# [Programmers] 여행경로

Problem URL : [여행경로](hhttps://programmers.co.kr/learn/courses/30/lessons/43164#)

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <map>

using namespace std;

map<string, vector<string>> m;
map<string, vector<bool>> visit;
int nums;
vector<string> route;
bool dfs(int idx) {
    if(idx == nums) return true;
    string now = route[idx];
    vector<string> tmp = m[now];
    for(int i = 0; i < tmp.size(); i++) {  //[1]
        if(visit[now][i]) continue;
        visit[now][i] = true;
        route.push_back(tmp[i]);
        if(dfs(idx + 1)) {
            return true;
        }
        route.pop_back();
        visit[now][i] = false;
    }
    return false;
}

vector<string> solution(vector<vector<string>> tickets) {
    sort(tickets.begin(), tickets.end());
    nums = tickets.size();
    string start = tickets[0][0];
    vector<string> ends;
    ends.push_back(tickets[0][1]);
    for(int i = 1; i < nums; i++) {
        if(start != tickets[i][0]) {
            m[start] = ends;
            int len = ends.size();
            vector<bool> tmp(len, false);
            visit[start] = tmp;
            ends.clear();
            start = tickets[i][0];
        }
        ends.push_back(tickets[i][1]);
    }
    m[start] = ends;
    int len = ends.size();
    vector<bool> tmp(len, false);
    visit[start] = tmp;
    route.push_back("ICN");
    dfs(0);
    return route;
}
```
## Comments
map을 사용하지 않고 [1] 전체 ticket 목록을 탐색하는 방법도 있다.  
그 방법이 코드 짜기는 더 편하지만, ticket 목록이 커질 수록  
map을 사용해주는게 효율이 2배는 좋아지는 거 같다. 