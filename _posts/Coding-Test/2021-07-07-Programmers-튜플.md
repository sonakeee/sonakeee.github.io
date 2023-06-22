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

title: "[Programmers] 튜플"
excerpt: "[Programmers] 튜플"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-07
last_modified_at: 2021-07-07
---
# [Programmers] 튜플

Problem URL : [튜플](https://programmers.co.kr/learn/courses/30/lessons/64065)


```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <set>

using namespace std;

bool cmp(vector<int> a, vector<int> b) {
    return a.size() < b.size();
}

vector<int> solution(string s) {
    vector<int> answer;
    vector<vector<int>> v;
    int size = s.size();
    vector<int> input;
    string tmp = "";
    bool open = false;
    for (int i = 1; i < size - 1; i++) {
        if (s[i] == '{') {
            open = true;
        }
        if (s[i] >= '0' && s[i] <= '9') {
            tmp += s[i];
        }
        if (s[i] == ',') {
            if (open) {
                input.push_back(stoi(tmp));
                tmp = "";
            }
        }
        if (s[i] == '}') {
            input.push_back(stoi(tmp));
            tmp = "";
            v.push_back(input);
            input.clear();
            open = false;
        }
    }

    sort(v.begin(), v.end(), cmp);
    
    set<int> tuple;
    for (int i = 0; i < v.size(); i++) {
        for (int j = 0; j < v[i].size(); j++) {
            if (tuple.find(v[i][j]) == tuple.end()) {
                tuple.insert(v[i][j]);
                answer.push_back(v[i][j]);
            }
        }
    }

    return answer;
}
```