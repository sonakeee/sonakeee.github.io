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

title: "[Programmers] 후보키"
excerpt: "[Programmers] 후보키"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 팰린드롬]
date: 2021-05-12
last_modified_at: 2021-05-12
---
# [Programmers] 후보키

Problem URL : [후보키](https://programmers.co.kr/learn/courses/30/lessons/42890)

```cpp
#include<vector>
#include<map>
#include<string>

using namespace std;

vector<int> keys;

bool isMinimal(int value) {
    for (const int& i : keys) {
        if ((value & i) == i)
            return false;
    }
    return true;
}

int solution(vector<vector<string>> r) {
    int ans = 0;
    int row = (int)r.size(); 
    int column = (int)r[0].size(); 

    for (int i = 1; i < (1 << column); i++) {
        // 각각의 i에 대해 최소성이 만족하는지는
        // i보다 작은 모든 값들과 비교해서 isMinimal
        // 메소드로 확인해주면 된다.
        // 작은 i값부터 확인해서 keys에 넣어진다!
        if (!isMinimal(i)) {
            continue;
        }

        map<string, int> m;
        for (int j = 0; j < row; j++) {
            // 튜플의 key에 해당하는 속성값들
            string studentInfo = ""; 
            for (int k = 0; k < column; k++) {
                if (i & (1 << k)) {
                    studentInfo += r[j][k];
                }
            } 
            m[studentInfo]++;
            if (m[studentInfo] > 1) {
                break;
            }
        }

        if (m.size() == row) {
            keys.push_back(i);
        }
    }

    ans = (int)keys.size();
    return ans;
}
```