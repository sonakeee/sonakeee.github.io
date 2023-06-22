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

title: "[Programmers] 파일명 정렬"
excerpt: "[Programmers] 파일명 정렬"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-04
last_modified_at: 2021-07-04
---
# [Programmers] 파일명 정렬

Problem URL : [파일명 정렬](https://programmers.co.kr/learn/courses/30/lessons/17684)


```cpp
#include <string>
#include <vector>
#include <map>
#include <algorithm>
#include <string>
#include <iostream>

using namespace std;

struct file {
    string head, full_name;
    int number;
    file(string h, string f, int n) : head(h), full_name(f), number(n) {};
};

bool cmp(file a, file b) {
    if (a.head == b.head) {
        return a.number < b.number;
    } else {
        return a.head < b.head;
    }
}

vector<string> solution(vector<string> files) {
    vector<string> answer;
    string head, number ;
    bool numbering;
    vector<file> v;

    for (int i = 0; i < files.size(); i++) {
        pair<string, int> p;
        head = number = "";
        numbering = false;
        for (int j = 0; j < files[i].size(); j++) {
            if (files[i][j] >= '0' && files[i][j] <= '9') {
                number += files[i][j];
                numbering = true;
            } else {
                if (numbering) {
                    break;
                } else {
                    head += tolower(files[i][j]);
                }
            }
        }
        file f(head, files[i], stoi(number));
        v.push_back(f);
    }

    stable_sort(v.begin(), v.end(), cmp);

    for (auto itr : v)
        answer.push_back(itr.full_name);

    return answer;
}
```

## Comments  
stable sort 활용법에 대해 알아두자