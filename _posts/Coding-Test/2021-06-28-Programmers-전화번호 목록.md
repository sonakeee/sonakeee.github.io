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

title: "[Programmers] 전화번호 목록"
excerpt: "[Programmers] 전화번호 목록"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, DFS]
date: 2021-06-27
last_modified_at: 2021-06-27
---
# [Programmers] 전화번호 목록

Problem URL : [전화번호 목록](https://programmers.co.kr/learn/courses/30/lessons/42577)

## 풀이 1 (char 형 이용)

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

const int SIZE = 10;

struct Trie
{
    Trie* next[SIZE];
    bool isFinished;
    bool hasNextChild;
    Trie()
    {
        fill(next, next + SIZE, nullptr);
        isFinished = hasNextChild = false;
    }

    ~Trie()
    {
        for (int i = 0; i < SIZE; i++) {
            if (next[i]) {
                delete next[i];
            }
        }
    }

    bool insert(const char* key)
    {
        if (*key == '\0')
        {
            isFinished = true;
            return !hasNextChild;
        } else if (isFinished) {
            return 0;
        }

        hasNextChild = true;
        int nextKey = *key - '0';

        if (!next[nextKey]) {
            next[nextKey] = new Trie;
        }
        return next[nextKey]->insert(key + 1);
    }
};

bool solution(vector<string> phone_book) {
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    Trie* root = new Trie;
    bool consistency = true;

    for (int i = 0; i < phone_book.size(); i++)
    {
        if (consistency && root->insert(&phone_book[i][0]) == 0) {
            consistency = false;
        }
    }

    delete root;

    return consistency;
}
```
## 풀이 2 (string 이용)
```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

const int SIZE = 10;

struct Trie
{
    Trie* next[SIZE];
    bool isFinished;
    bool hasNextChild;

    Trie(){
        fill(next, next + SIZE, nullptr);
        isFinished = hasNextChild = false;
    }

    ~Trie(){
        for (int i = 0; i < SIZE; i++) {
            if (next[i]) {
                delete next[i];
            }
        }
    }

    bool insert(string phone_num, int index){
        if (phone_num[index] == '\0')
        {
            isFinished = true;
            return !hasNextChild;
        } else if (isFinished) {
            return 0;
        }

        hasNextChild = true;
        int nextKey = phone_num[index] - '0';

        if (!next[nextKey]) {
            next[nextKey] = new Trie;
        }
        return next[nextKey] -> insert(phone_num, index + 1);
    }
};



bool solution(vector<string> phone_book) {
    ios::sync_with_stdio(false); cin.tie(NULL); cout.tie(NULL);
    Trie* root = new Trie;
    bool consistency = true;

    for (int i = 0; i < phone_book.size(); i++)
    {
        if (consistency && root -> insert(phone_book[i],0) == 0) {
            consistency = false;
        }
    }
    delete root;
    return consistency;
}
```

## Comments
char 형으로 푸는 것이 속도가 2배는 빠른 거 같다.