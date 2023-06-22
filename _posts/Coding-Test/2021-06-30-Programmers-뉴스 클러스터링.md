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

title: "[Programmers] 뉴스 클러스터링"
excerpt: "[Programmers] 뉴스 클러스터링"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-06-30
last_modified_at: 2021-06-30
---
# [Programmers] 뉴스 클러스터링

Problem URL : [뉴스 클러스터링](https://programmers.co.kr/learn/courses/30/lessons/17677?language=cpp)


```cpp
#include <string>
#include <algorithm>
#include <iostream>
#include <unordered_map>

using namespace std;

int solution(string str1, string str2) {
    int size1, size2, ans = 65536;
    double all = 0, dup = 0;
    unordered_map<string, int> um;

    transform(str1.begin(), str1.end(), str1.begin(), ::tolower);
    transform(str2.begin(), str2.end(), str2.begin(), ::tolower);

    size1 = str1.size();
    size2 = str2.size();

    for (int i = 0; i < size1 - 1; i++) {
        if (str1[i] < 97 || str1[i] > 122) continue;
        if (str1[i + 1] < 97 || str1[i + 1] > 122) continue;

        string str(1, str1[i]);
        str += str1[i + 1];
        all++;

        um[str]++;
    }

    for (int i = 0; i < size2 - 1; i++) {
        if (str2[i] < 97 || str2[i] > 122) continue;
        if (str2[i + 1] < 97 || str2[i + 1] > 122) continue;

        string str(1, str2[i]);
        str += str2[i + 1];
        
        // str1에서 중복된 경우는 합집합에 기여하지 않고
        // 교집합에만 기여한다.
        if (um[str] > 0) {
            um[str]--;
            dup++;
        } else {
            all++;
        }
    }

    if (all != 0) {
        ans = (int)(ans * (dup / all));
    }

    return ans;
}
```