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

title: "[Programmers] 조이스틱"
excerpt: "[Programmers] 조이스틱"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열, 그리디]
date: 2021-06-26
last_modified_at: 2021-06-26
---
# [Programmers] 조이스틱

Problem URL : [조이스틱](https://programmers.co.kr/learn/courses/30/lessons/42860)

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(string name) {
    int answer = 0;
    int size = name.length();
    // 세로 조작 횟수
    for(int i = 0; i < size; i++) {
        int a = name[i] - 'A';
        int b = 'Z' - name[i] + 1;
        if (a > b) {
            answer += b;
        }else{
            answer += a;
        }
    }
    
    // 가로 조작횟수 (최대 size - 1)
    int add = size - 1;
    for(int i = 0; i < size; i++) {
        int count = 0; // 연속된 A의 개수
        int j = 0;
        if(name[i] == 'A'){
            count ++;
            while(i + j + 1 < size && name[i + j + 1] == 'A'){
                j++;
                count++;
            }
            int tmp;
            if(i == 0){
                // 첫자리가 A일 때
                tmp = size - 1 - count;
            }else{
                int a = size - 1 - count + (i - 1); // 오른쪽으로 먼저 이동했을 때
                int b = size - 1 - count + (size - i - count); // 왼쪽으로 먼저 이동했을 때
                if(a < b){
                    tmp = a;
                }else {
                    tmp = b;
                }
            }
    
            if(add > tmp) {
                add = tmp;
            }
            i += j;
        }    
    }
    answer += add;
    return answer;
}
```
