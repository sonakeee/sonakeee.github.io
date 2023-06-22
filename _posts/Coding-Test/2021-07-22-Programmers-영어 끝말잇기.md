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

title: "[Programmers] 영어 끝말잇기"
excerpt: "[Programmers] 영어 끝말잇기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-07-22
last_modified_at: 2021-07-22
---
# [Programmers] 영어 끝말잇기

Problem URL : [영어 끝말잇기](https://programmers.co.kr/learn/courses/30/lessons/42578)

```cpp
#include <string>
#include <vector>
#include <iostream>
#include <unordered_map>

using namespace std;

vector<int> solution(int n, vector<string> words) {
    vector<int> answer;
    unordered_map<string, int> m;
    int size = words.size();
    int count = 1;
    char last = words[0][0];
    for(int i = 0; i < size; i++) {
        string word = words[i];
        if(last != words[i][0] || m[word] == 1){
            break;
        }
        m[word] = 1;
        int length = word.size();
        last = word[length - 1];
        count++;
    }
    if(count > size) {
        answer.push_back(0);
        answer.push_back(0);
        return answer;
    }else{
        int q;
        int r = count % n;
        if(r == 0) {
            r = n;
            q = count / n;
        }else {
            q = count / n + 1;
        }
        answer.push_back(r);
        answer.push_back(q);
        return answer;
    }
}
```

## Comments

|              | unordered_map         | map                   |
| ------------ | --------------------- | --------------------- |
| 테스트 1 〉  | 통과 (0.01ms, 3.95MB) | 통과 (0.02ms, 3.97MB) |
| 테스트 2 〉  | 통과 (0.02ms, 3.94MB) | 통과 (0.02ms, 3.95MB) |
| 테스트 3 〉  | 통과 (0.03ms, 3.94MB) | 통과 (0.01ms, 3.94MB) |
| 테스트 4 〉  | 통과 (0.02ms, 3.97MB) | 통과 (0.02ms, 3.96MB) |
| 테스트 5 〉  | 통과 (0.03ms, 3.98MB) | 통과 (0.03ms, 3.95MB) |
| 테스트 6 〉  | 통과 (0.02ms, 3.98MB) | 통과 (0.02ms, 3.93MB) |
| 테스트 7 〉  | 통과 (0.02ms, 3.82MB) | 통과 (0.02ms, 3.94MB) |
| 테스트 8 〉  | 통과 (0.01ms, 3.91MB) | 통과 (0.01ms, 3.96MB) |
| 테스트 9 〉  | 통과 (0.02ms, 3.94MB) | 통과 (0.02ms, 3.98MB) |
| 테스트 10 〉 | 통과 (0.03ms, 3.84MB) | 통과 (0.03ms, 3.96MB) |
| 테스트 11 〉 | 통과 (0.04ms, 3.98MB) | 통과 (0.03ms, 3.96MB) |
| 테스트 12 〉 | 통과 (0.02ms, 3.91MB) | 통과 (0.02ms, 3.94MB) |
| 테스트 13 〉 | 통과 (0.02ms, 3.94MB) | 통과 (0.01ms, 3.94MB) |
| 테스트 14 〉 | 통과 (0.01ms, 3.95MB) | 통과 (0.01ms, 3.96MB) |
| 테스트 15 〉 | 통과 (0.01ms, 3.9MB)  | 통과 (0.01ms, 3.95MB) |
| 테스트 16 〉 | 통과 (0.02ms, 3.95MB) | 통과 (0.02ms, 3.98MB) |
| 테스트 17 〉 | 통과 (0.01ms, 3.94MB) | 통과 (0.01ms, 3.96MB) |
| 테스트 18 〉 | 통과 (0.01ms, 3.95MB) | 통과 (0.02ms, 3.93MB) |
| 테스트 19 〉 | 통과 (0.01ms, 3.98MB) | 통과 (0.01ms, 3.95MB) |
| 테스트 20 〉 | 통과 (0.04ms, 3.96MB) | 통과 (0.05ms, 3.96MB) |

unordered_map이랑 map을 썼을 때 크게 차이는 안난다.

제일 느린 경우는 unordered_map이 조금 더 좋아보이지만, map보다 느린 테스트 케이스도 있다.
