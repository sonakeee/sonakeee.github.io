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

title: "[Programmers] [3차]압축"
excerpt: "[Programmers] [3차]압축"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-03
last_modified_at: 2021-07-03
---
# [Programmers] [3차]압축

Problem URL : [[3차]압축](https://programmers.co.kr/learn/courses/30/lessons/17684)

## 내 풀이

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(string msg) {
    vector<int> answer;
    vector<string> sList;
    sList.push_back("#");
    for (int i = 65; i <= 90; i++) {
        string s;
        s = (char)i;
        sList.push_back(s);
    }
    int index = 0;
    int size = msg.size();
    while(index < size) {
        for(int i = size - index; i >= 1; i--) {
            auto iter = find(sList.begin(), sList.end(), msg.substr(index, i));
            if(iter != sList.end()) {
                int idx = distance(sList.begin(), iter);
                answer.push_back(idx);
                index += sList[idx].size();
                sList.push_back(sList[idx] + msg[index]);
                break;
            }
        }
    }
    return answer;
}
```

## 모범 답안

```cpp
#include <string>
#include <vector>

using namespace std;

int search(vector<string> &dictionary, string msg)
{
    int len = dictionary.size();
    for (int i = 1; i < len; i++)
    {
        if (msg == dictionary[i])
            return i;
    }
    return -1;
}

vector<int> solution(string msg) 
{
    vector<int> answer;
    int len = msg.length();
    vector<string> dictionary = { "-1" };

    for (int i = 0; i < 26; i++)
    {
        dictionary.push_back(string(1, 'A' + i));
    }

    int cur = 0;
    int checkLen = 1;
    int idx = -1;

    while (true) 
    {
        string str = msg.substr(cur, checkLen);     
        int tmp = search(dictionary, str);

        if (tmp > 0)//hit
        {
            idx = tmp;
            checkLen++;
            if (cur + checkLen - 1 == len)
            {
                answer.push_back(idx);
                break;
            }
        }
        else//miss
        {
            answer.push_back(idx);
            dictionary.emplace_back(str);
            cur += checkLen-1;
            checkLen = 1;
            idx = -1;
        }
    }
    return answer;
}
```