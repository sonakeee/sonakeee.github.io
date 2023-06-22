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

title: "[Programmers] 메뉴 리뉴얼"
excerpt: "[Programmers] 메뉴 리뉴얼"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 조합, DFS, 문자열]
date: 2021-06-29
last_modified_at: 2021-06-29
---
# [Programmers] 메뉴 리뉴얼

Problem URL : [메뉴 리뉴얼](https://programmers.co.kr/learn/courses/30/lessons/72411)


```cpp
#include <string>
#include <vector>
#include <unordered_map>
#include <algorithm>
using namespace std;

int maxNum[11]; 
unordered_map<string, int> um; 
vector<string> menu[11]; 

void dfs(string s, int idx, string made) {
    int size = made.size();
    if (size > 1) { 
        um[made]++; 
        if (maxNum[size] == um[made]) {
            menu[size].push_back(made);
        }
        if (maxNum[size] < um[made]) {
            maxNum[size] = um[made];
            menu[size].clear();
            menu[size].push_back(made);
        }
    }

    for (int i = idx + 1; i < s.size(); i++) {
        made.push_back(s[i]);
        dfs(s, i, made);
        made.pop_back();
    }
}
vector<string> solution(vector<string> orders, vector<int> course) {
    vector<string> answer;

    for (string& s : orders) {
        sort(s.begin(), s.end()); 
        dfs(s, -1, "");
    }

 
    for (int i : course)
        if (maxNum[i] > 1) 
            for (string s : menu[i])
                answer.push_back(s);

    sort(answer.begin(), answer.end());

    return answer;
}
```