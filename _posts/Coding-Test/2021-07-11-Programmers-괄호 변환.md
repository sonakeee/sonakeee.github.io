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

title: "[Programmers] 괄호 변환"
excerpt: "[Programmers] 괄호 변환"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열, DFS]
date: 2021-07-11
last_modified_at: 2021-07-11
---
# [Programmers] 괄호 변환

Problem URL : [괄호 변환](https://programmers.co.kr/learn/courses/30/lessons/60058)

```cpp
#include <string>
#include <vector>

using namespace std;

string flip(string u) {
    string ret = "";
    for (int i = 0; i < u.size(); i++) {
        if (u[i] == '(') {
            ret += ")";
        } else {
            ret += "(";
        }
    }
    return ret;
}

int count(char bracket) {
    if (bracket == '(') {
        return 1;
    }
    return -1;
}

string dfs(string p) {
    if (p == "") {
        return p;
    }
    int sum = 0;
    sum += count(p[0]);
    int idx = 1;
    bool proper = true;
    while (sum != 0) {
        if (sum < 0) {
            proper = false;
        }
        sum += count(p[idx]);
        idx++;
    }
    string u = p.substr(0, idx);
    string v = p.substr(idx);
    if (proper) {
        return u + dfs(v);
    } else {
        return "(" + dfs(v) + ")" + flip(u.substr(1,u.size() - 2));
    }
}

string solution(string p) {
    return dfs(p);
}
```