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

title: "[Programmers] 타겟 넘버"
excerpt: "[Programmers] 타겟 넘버"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, DFS]
date: 2021-06-27
last_modified_at: 2021-06-27
---
# [Programmers] 타겟 넘버

Problem URL : [타겟 넘버](https://programmers.co.kr/learn/courses/30/lessons/43165)

```cpp
#include <string>
#include <vector>

using namespace std;
int n;
int answer;
int t;
void dfs(vector<int> &numbers, int index, int sum) {
    if (index == n) {
        if(sum == t){
            answer++;
        }
        return;
    }
    dfs(numbers, index + 1, sum + numbers[index]);
    dfs(numbers, index + 1, sum - numbers[index]);
}

int solution(vector<int> numbers, int target) {
    n = numbers.size();
    t = target;
    answer = 0;
    dfs(numbers, 0, 0);
    return answer;
}
```
