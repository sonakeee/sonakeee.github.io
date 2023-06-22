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

title: "[Programmers] [1차]캐시"
excerpt: "[Programmers] [1차]캐시"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-06
last_modified_at: 2021-07-06
---
# [Programmers] [1차]캐시

Problem URL : [[1차]캐시](https://programmers.co.kr/learn/courses/30/lessons/17680)

## 내 풀이

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(int cacheSize, vector<string> cities) {
    int answer = 0;
    vector<string> q;

    for (int i = 0; i < cities.size(); i++) {
        string city = cities[i];
        transform(city.begin(), city.end(), city.begin(), ::tolower);
        int idx;
        auto it = find(q.begin(), q.end(), city);
        if (it != q.end()) {
            q.erase(it);
            q.push_back(city);
            answer++;
        }else {
            if (cacheSize != 0) {
                if (q.size() == cacheSize) {
                    q.erase(q.begin());
                }
                q.push_back(city);
            }
            answer += 5;
        }
    }
    return answer;
}
```

## 모범답안

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;
int solution(int cacheSize, vector<string> cities) {

    vector <string> q;
    int duration = 0;

    for(vector <string>::iterator it = cities.begin(); it != cities.end(); it++){
        transform(it->begin(), it->end(), it->begin(), ::tolower);

        bool flag = false;
        for(vector<string>::iterator itt = q.begin(); itt != q.end(); itt++){
            if(*itt == *it) {
                flag = true;
                duration +=1;
                q.erase(itt);
                q.push_back(*it);
                break;
            }
        }
        if(!flag) {
            duration +=5;
            if(cacheSize != 0 && q.size() >= cacheSize)
                q.erase(q.begin());
            if(q.size() < cacheSize)
                q.push_back(*it);
        }
    }

    return duration;
}
```