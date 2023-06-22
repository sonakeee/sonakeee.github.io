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

title: "[Programmers] JadenCase 문자열 만들기"
excerpt: "[Programmers] JadenCase 문자열 만들기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열, 파싱]
date: 2021-07-17
last_modified_at: 2021-07-17
---
# [Programmers] JadenCase 문자열 만들기

Problem URL : [JadenCase 문자열 만들기](https://programmers.co.kr/learn/courses/30/lessons/12951)

## 풀이 1(sstream 을 이용한 풀이)

```cpp
#include <string>
#include <vector>
#include <cstring>
#include <sstream>

using namespace std;

string solution(string s) {
    istringstream ss(s);
    string stringBuffer;
    vector<string> x;
    
    while (getline(ss, stringBuffer, ' ')){
        stringBuffer[0] = toupper(stringBuffer[0]);
        for (int i = 1; i < stringBuffer.size(); i++) {
            stringBuffer[i] = tolower(stringBuffer[i]);
        }
        x.push_back(stringBuffer);
    }
    string answer = "";
    answer = x[0];
    for(int i = 1; i < x.size(); i++) {
        answer += " ";
        answer += x[i];
    }
    // 마지막 공백부분 체크
    if(answer.size() < s.size()) {
        answer += s.substr(answer.size(), s.size() - answer.size());
    }
    return answer;
}
```

## 풀이 2 (단순 깔금한 풀이)
```cpp
#include <string>
#include <vector>

using namespace std;

string solution(string s) {
    int size = s.size();
    bool blank = true;
    for(int i = 0; i < size; i++) {
        if(blank) {
            s[i] = toupper(s[i]);
            // 공백이 연속될 수도 있다.
            if(s[i] != ' ') {
                blank = false;
            }
        }else{
            s[i] = tolower(s[i]);   
            if(s[i] == ' ') {
                blank = true;
            }
        }
    }
    return s;
}
```

## Comments  
당연하지만 풀이 2가 훨신 빠르다.   
게다가 깔금하다.