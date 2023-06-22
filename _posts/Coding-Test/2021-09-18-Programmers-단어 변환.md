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

title: "[BOJ] 단어 변환"
excerpt: "[BOJ] 단어 변환"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, DFS]
date: 2021-09-18
last_modified_at: 2021-09-18
---
# [BOJ] 단어 변환

Problem URL : [단어 변환](https://programmers.co.kr/learn/courses/30/lessons/43163)

```cpp
#include <string>
#include <vector>
using namespace std;
int answer;
int num;
int len;
 
void dfs(string begin, string target, vector<string> &words, vector<bool> &visit, int cnt) {
    if(answer <= cnt) {
        return;  // 조기종료
    }
    if(begin == target && answer > cnt) {
        answer = cnt;
        return;
    }
    for (int i = 0; i < num; i++) {
        if(visit[i]) continue;  // 방문했던 word는 pass
        int diff = 0;
        for (int j = 0; j < len; j++)
            if (begin[j] != words[i][j]) diff++;
        if (diff == 1) {  // 한글자만 다를 때 계속 탐색
            visit[i] = true;
            dfs(words[i], target, words, visit, cnt + 1);
            visit[i] = false;
        }
    }
}
 
int solution(string begin, string target, vector<string> words) {
    answer = 51;
    num = words.size();
    len = words[0].size();
    vector<bool> visit(num, false);
    dfs(begin, target, words, visit, 0);
    if (answer == 51) return 0;
    return answer;
}
```
