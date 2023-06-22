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

title: "[Programmers] 징검다리 건너기"
excerpt: "[Programmers] 징검다리 건너기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 구현, 파라메트릭 서치]
date: 2021-10-11
last_modified_at: 2021-10-12
---
# [Programmers] 징검다리 건너기

Problem URL : [징검다리 건너기](https://programmers.co.kr/learn/courses/30/lessons/64062)

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool bs(int &mid, int &k, vector<int> &v){
    int cnt = 0;
    for(int i = 0; i < v.size(); i++){
        if(v[i] - mid <= 0)
            cnt++;
        else
            cnt = 0;
        if(cnt >= k)
            return false;
    }
    return true;
}

int solution(vector<int> stones, int k) {
    int start = 1;
    int end = 0;
    int answer = 0;
    for(int i = 0; i < stones.size(); i++) {
        if(end < stones[i]) {
            end = stones[i];
        }
    }
    while(start <= end){
        int mid = (start + end) / 2;
        if(bs(mid, k, stones)) { // mid만큼 건너고도 아직 더 건널 수 있을 때
            answer = mid; 
            start = mid + 1;
        }else
            end = mid - 1;
    }

    // answer는 더이상 건널 수 없는 상태 직전까지의 값이 저장되어있다.
    return answer + 1;
}
```

## Comments
M = 배열의 길이, N은 디딤돌 최대값 이라고 할 때  
파라메트릭 서치를 이용하면 O(Mlog(N)) 만에 가능하다.

다음은 효율성 테스트에서 시간 초과한 풀이다.  
[1]의 시간복잡도가 최악의 경우 O(MN)이어서 효율성을 통과하지 못한다.

```cpp
#include <string>
#include <vector>
#include <set>
#include <unordered_map>
#include <iostream>

using namespace std;

int solution(vector<int> stones, int k) {
    int answer = 0;
    set<int> s;
    unordered_map<int,vector<int>> um;
    for(int i = 0; i < stones.size(); i++) {
        s.insert(stones[i]);
        um[stones[i]].push_back(i);
    }
    
    set<int> zero;
    zero.clear();
    int val;
    // [1]
    for(auto it = s.begin(); it != s.end(); it++){
        val = *it;
        for(int i = 0; i < um[*it].size(); i++) {
            zero.insert(um[*it][i]);
        }
        int pre = -1;
        int cnt = 0;
        for(auto iter = zero.begin(); iter != zero.end(); iter++) {
            if(*iter == pre + 1) {
                cnt++;
            }else {
                cnt = 1;
            }
            pre = *iter;
            if(cnt == k) return *it;
        }
	}
    return val;
}
```