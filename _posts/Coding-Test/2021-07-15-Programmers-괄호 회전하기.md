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

title: "[Programmers] 괄호 회전하기"
excerpt: "[Programmers] 괄호 회전하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, Stack, 문자열]
date: 2021-07-15
last_modified_at: 2021-07-15
---
# [Programmers] 괄호 회전하기

Problem URL : [괄호 회전하기](https://programmers.co.kr/learn/courses/30/lessons/76502)

```cpp
#include <string>
#include <vector>
#include <stack>

using namespace std;
int len;

bool proper(string s) {
    bool proper = true;
    stack<char> st;
    for (int i = 0; i < len; i++) {

      if(s[i] == '(' || s[i] == '{' || s[i] == '[') {
          st.push(s[i]);
      }else {
          
          if(st.empty()){
            proper = false;
            break;
          }else if(s[i] == ')') {
            if(st.top() == '('){
              st.pop();
            }else{
              proper = false;
              break;
            }
          }else if(s[i] == '}') {
            if(st.top() == '{'){
              st.pop();
            }else{
              proper = false;
              break;
            }
          }else {
            if(st.top() == '['){
              st.pop();
            }else{
              proper = false;
              break;
            }
          }
        
      }
        
    }
    if(!st.empty()) {
      proper = false;
    }
    return proper;
}

int solution(string s) {
    int cnt = 0;
    len = s.size();
    for (int i = 0; i < len; i++) {
        if(proper(s)) {
            cnt ++;
        }
        char f = s[0];
        s = s.substr(1,len - 1);
        s = s + f;
    }
    
    return cnt;
}
```