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

title: "[Programmers] 문자열 압축"
excerpt: "[Programmers] 문자열 압축"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열, 구현]
date: 2021-06-25
last_modified_at: 2021-06-25
---
# [Programmers] 문자열 압축

Problem URL : [문자열 압축](https://programmers.co.kr/learn/courses/30/lessons/60057)

```cpp
#include <string>

using namespace std;

int solution(string s) {
    int answer = s.length();
    // 압축 길이는 최소 1부터 최대 전체 문자열의 절반이다.
    for (int i = 1; i <= s.length() / 2; i++) {
        int len = s.length();
        int count = 0;
        // 맨 앞에서부터 i씩 잘라준다.
        for (int j = i; j < s.length(); j += i) {
            if (s.substr(j, i) == s.substr(j - i, i)) {
                count++;
            } else {
                //중복이 있으면 압축하고 축소된 길이 계산
                if (count) {
                    len -= i * count;
                    len += to_string(count + 1).length();
                }
                count = 0;
            }
        }
        //중복이 있으면 압축하고 축소된 길이 계산
        if (count) {
            len -= i * count;
            len += to_string(count + 1).length();
        }

        if (len < answer) {
            answer = len;
        }
    }
    return answer;
}
```
