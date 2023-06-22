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

title: "[Programmers] 최댓값과 최솟값"
excerpt: "[Programmers] 최댓값과 최솟값"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 파싱]
date: 2021-07-17
last_modified_at: 2021-07-17
---
# [Programmers] 최댓값과 최솟값

Problem URL : [최댓값과 최솟값](https://programmers.co.kr/learn/courses/30/lessons/12939)

## 풀이 1

```cpp
#include <string>
#include <vector>
#include <sstream>
#include <algorithm>
#include <iostream>

using namespace std;

string solution(string s) {
    string answer = "";
    istringstream iss(s);
    vector<int> v;
    string _s;
    while(iss >> _s) {
        v.push_back(stoi(_s));
    }
    sort(v.begin(), v.end());
    answer = to_string(v[0]) + " "  + to_string(v[v.size()-1]);
    return answer;
}
```

## 풀이 2

```cpp
#include <string>
#include <vector>
#include <sstream>
#include <algorithm>
#include <iostream>

using namespace std;

string solution(string s) {
    string answer = "";
    istringstream iss(s);
    vector<int> v;
    int n;
    while(iss >> n) {
        v.push_back(n);
    }
    sort(v.begin(), v.end());
    answer = to_string(v[0]) + " " + to_string(v[v.size()-1]);
    return answer;
}
```

## Comments
istringstream은 바로 정수형으로도 파싱해준다!
