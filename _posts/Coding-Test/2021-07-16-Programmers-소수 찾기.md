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

title: "[Programmers] 소수 찾기"
excerpt: "[Programmers] 소수 찾기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 순열, 소수]
date: 2021-07-16
last_modified_at: 2021-07-16
---
# [Programmers] 소수 찾기

Problem URL : [소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)

## 풀이1 (Unordered_map을 이용한 풀이)

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <cmath>
#include <unordered_map>
 
using namespace std;
unordered_map<int, bool> um;

bool isPrime(int n) {
    if (n < 2) return false;
    int root = sqrt(n);
    for (int i = 2; i <= root; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
 
int solution(string numbers) {
    int answer = 0;
    sort(numbers.begin(), numbers.end());
    do {
        string temp = "";
        for (int i = 0; i < numbers.size(); i++) {
            temp.push_back(numbers[i]);
            int tmp = stoi(temp);
            if(!um[tmp]) {
                um[tmp] = true;
                if(isPrime(tmp)){
                    answer++;
                }
            }
        }
    } while (next_permutation(numbers.begin(), numbers.end()));
 
    return answer;
}
```

## 풀이 2(Unordered_set을 이용한 풀이)

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <cmath>
#include <unordered_set>
 
using namespace std;
unordered_set<int> us;

bool isPrime(int n) {
    if (n < 2) return false;
    int root = sqrt(n);
    for (int i = 2; i <= root; i++) {
        if (n % i == 0) {
            return false;
        }
    }
 
    return true;
}
 
 
int solution(string numbers) {
    sort(numbers.begin(), numbers.end());
    do {
        string temp = "";
        for (int i = 0; i < numbers.size(); i++) {
            temp.push_back(numbers[i]);
            int tmp = stoi(temp);
            if(isPrime(tmp)){
              us.insert(tmp);
            }
        }
    } while (next_permutation(numbers.begin(), numbers.end()));
 
 
    return us.size();
}
```

## Comments  
풀이 2가 풀이 1보다 조금 더 빠르다.  
순열 활용 방법과 unordered_set, unorded_map 활용법을 잘 익혀두자