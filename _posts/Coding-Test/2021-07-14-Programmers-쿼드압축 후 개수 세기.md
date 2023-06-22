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

title: "[Programmers] 쿼드압축 후 개수 세기"
excerpt: "[Programmers] 쿼드압축 후 개수 세기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현, DFS]
date: 2021-07-14
last_modified_at: 2021-07-14
---
# [Programmers] 쿼드압축 후 개수 세기

Problem URL : [쿼드압축 후 개수 세기](https://programmers.co.kr/learn/courses/30/lessons/68936)

## 풀이 1 (작은 사각형부터 큰 사각형 순으로 dfs)

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;
int one = 0;
int zero;
int n;

void dfs(vector<vector<int>> &arr, int size) {
    if(n == size) {
        return;
    }
    for(int i = 0; i < n / size; i = i + 2) {
        for(int j = 0; j < n / size; j = j + 2) {
            if(arr[i][j] == 1 && arr[i][j + 1] == 1 && arr[i + 1][j] == 1 && arr[i + 1][j + 1] == 1){
                arr[i/2][j/2] = 1;
                one = one - 3;
            }else if(arr[i][j] == 0 && arr[i][j + 1] == 0 && arr[i + 1][j] == 0 && arr[i + 1][j + 1] == 0){
                arr[i/2][j/2] = 0;
                zero = zero - 3;
            }else {
                arr[i/2][j/2] = 2;
            }
        }
    }
    dfs(arr, 2 * size);
}

vector<int> solution(vector<vector<int>> arr) {
    vector<int> answer;
    n = arr.size();
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(arr[i][j] == 1) {
                one++;
            }
        }
    }
    zero = n * n - one;
    if(zero != 0 && one != 0){
        dfs(arr,1);
        answer.push_back(zero);
        answer.push_back(one);
    }else {
        if(one == 0) {
            answer.push_back(1);
            answer.push_back(0);
        }else {
            answer.push_back(0);
            answer.push_back(1);
        }
    }
    return answer;
}
```

## 풀이 2 (큰 사각형부터 작은 사각형 순으로 dfs)

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;
int one = 0;
int zero;
int n;

void dfs(vector<vector<int>> &arr, int x, int y, int size) {
    if(size <= 1) {
        return;
    }
    int tmp = arr[x][y];
    bool zip = true;
    for(int i = x; i < x + size; i++) {
        for(int j = y; j < y + size; j++) {
            if(tmp != arr[i][j]) {
                zip = false;
                break;
            }
        }
    }
    if(zip) {
        if(tmp == 0) {
            zero = zero - size*size + 1;
        } else {
            one = one - size*size + 1;
        }
    }else{
        dfs(arr, x, y, size / 2);
        dfs(arr, x, y + size / 2, size / 2);
        dfs(arr, x + size / 2, y, size / 2);
        dfs(arr, x + size / 2, y + size / 2, size / 2);
    }
}

vector<int> solution(vector<vector<int>> arr) {
    vector<int> answer;
    n = arr.size();
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(arr[i][j] == 1) {
                one++;
            }
        }
    }
    zero = n * n - one;
    if(zero != 0 && one != 0) {
        dfs(arr, 0, 0, n / 2);
        dfs(arr, 0, n / 2, n / 2);
        dfs(arr, n / 2, 0, n / 2);
        dfs(arr, n / 2, n / 2, n / 2);
        answer.push_back(zero);
        answer.push_back(one);
    }else {
        if(one == 0) {
            answer.push_back(1);
            answer.push_back(0);
        }else {
            answer.push_back(0);
            answer.push_back(1);
        }
    }
    
    return answer;
}
```

## Comments  
풀이 1이 풀이 2보다 효율은 조금 더 좋지만, 구현은 2가 훨씬 쉽다!