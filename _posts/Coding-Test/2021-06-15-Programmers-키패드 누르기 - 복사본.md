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

title: "[Programmers] 키패드 누르기"
excerpt: "[Programmers] 키패드 누르기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 시뮬레이션]
date: 2021-06-15
last_modified_at: 2021-06-15
---
# [Programmers] 키패드 누르기

Problem URL : [키패드 누르기](https://programmers.co.kr/learn/courses/30/lessons/67256)

```cpp
#include <string>
#include <vector>
#include <cstdlib>
using namespace std;

string solution(vector<int> numbers, string hand) {
    string answer = "";
    int dx[] = {2, 1, 2, 3, 1, 2, 3, 1, 2, 3};
    int dy[] = {4, 1, 1, 1, 2, 2, 2, 3, 3, 3};
    int lx = 1, ly = 4, rx = 1, ry = 4;
    int ld, rd;
    bool right = false;
    if (hand == "right") {
        right = true;
    }
    for (int i : numbers) {
        if (i == 1 || i == 4 || i == 7) {
            answer.push_back('L');
            lx = dx[i];
            ly = dy[i];
        } else if (i == 3 || i == 6 || i == 9) {
            answer.push_back('R');
            rx = dx[i];
            ry = dy[i];
        }else {
            ld = abs(lx - dx[i]) + abs(ly - dy[i]);
            rd = abs(rx - dx[i]) + abs(ry - dy[i]);
            if (ld == rd) {
                if (right) {
                    answer.push_back('R');
                    rx = dx[i];
                    ry = dy[i];
                } else {
                    answer.push_back('L');
                    lx = dx[i];
                    ly = dy[i];
                }
            } else if (ld > rd) {
                answer.push_back('R');
                rx = dx[i];
                ry = dy[i];
            } else {
                answer.push_back('L');
                lx = dx[i];
                ly = dy[i];
            }
        } 
    }
    return answer;
}
```
