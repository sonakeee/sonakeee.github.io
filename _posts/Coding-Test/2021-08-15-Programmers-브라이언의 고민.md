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

title: "[Programmers] 브라이언의 고민"
excerpt: "[Programmers] 브라이언의 고민"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현, 문자열]
date: 2021-08-12
last_modified_at: 2021-08-12
---
# [Programmers] 브라이언의 고민

Problem URL : [브라이언의 고민](https://programmers.co.kr/learn/courses/30/lessons/1830#)

```cpp
#include <string>
#include <vector>
#include <map>
#define p pair<int, int>
#include <iostream>

using namespace std;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(string sentence) {
    string answer = "";
    string invalid = "invalid";
    int size = sentence.length();
    vector<bool> start;
    vector<bool> end;
    start.resize(size + 1);
    end.resize(size + 1);
    map<char,vector<int>> m;
    vector<int> series;

    int lowerCnt = 0;
    for(int i = 0; i < size; i++) {
        char c = sentence[i];
        if(islower(c)) {
            vector<int> tmp = m[c];
            tmp.push_back(i);
            m[c] = tmp;
            lowerCnt++;
            if(lowerCnt == 2) {
                series.push_back(i);
            }else if(lowerCnt > 2){
                return invalid;
            }
        }else {
            lowerCnt = 0;
        }
    }

    for(auto const &i : series) {
        vector<int> pre = m[sentence[i - 1]];
        vector<int> post = m[sentence[i]];
        if(pre.size() != 2 || post.size() != 2) {
            return invalid;
        }
        int a = pre[0];
        int b = pre[1];
        int c = post[0];
        int d = post[1];
        if((a < c && c < b) || (a < d && d < b) || (c < a && a < d) || (c < b && b < d)) {
            return invalid;
        }
    }

    for(auto const &i:m) {
        vector<int> tmp = i.second;
        int tSize = tmp.size();
        if(tSize != 2) {
            if(tmp[0] == 0 || tmp[tSize - 1] == size - 1) {
                return invalid;
            }
            for(int j = 0; j < tSize - 1; j++) {
                if(tmp[j + 1] - tmp[j] != 2) {
                    return invalid;
                }
            }
            if(start[tmp[0] - 1]|| start[tmp[tSize - 1] + 1]) {
                return invalid;
            }
            start[tmp[0] - 1] = true;
            end[tmp[tSize - 1] + 1] = true;
        }
    }

    map<char,bool> check;
    for(int i = 0; i < size; i++) {
        char c = sentence[i];
        if(islower(c)) {
            vector<int> tmp = m[c];
            if(tmp.size() == 2 && i == tmp[0] && !check[c]) {
                int a = tmp[0] + 1;
                int b = tmp[1] - 1;
                for(int j = a + 1; j < b; j++) {
                    if(start[j]) {
                        return invalid;
                    }
                    vector<int> tmp2 = m[sentence[j]];
                    if(tmp2.size() == 2) {
                        if( b - a == 4 && tmp2[0] == a + 1 && tmp2[1] == b - 1) {
                            check[sentence[j]] = true;
                        }else {
                            return invalid;
                        }
                    }
                }
                start[a] = true;
                end[b] = true;
            }
        }
    }
 

    for(int i = 0; i < size; i++) {
        if(isupper(sentence[i])) {
            if(start[i] && !answer.empty() && answer.substr(answer.size() -1) != " ") {
                answer += " ";
            }
            answer += sentence[i];
            if(end[i])  {
                answer += " ";
            }
        }
    }
    if(answer.substr(answer.size() -1) == " ") {
        answer = answer.substr(0,answer.size() - 1);
    }
    return answer;
}
```

## Comments
AC가 되지 않는다... 열심히 반례 케이스를 찾는 중 ㅠㅠ