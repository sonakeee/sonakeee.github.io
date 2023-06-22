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

title: "[BOJ] 표 편집"
excerpt: "[BOJ] 표 편집"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 구현, STACK, 연결 리스트]
date: 2021-09-04
last_modified_at: 2021-09-04
---
# [BOJ] 표 편집

Problem URL : [표 편집](https://programmers.co.kr/learn/courses/30/lessons/81303)

## set을 이용한 풀이
```cpp
#include <set>
#include <iterator>
#include <string>
#include <vector>
#include <stack>
using namespace std;

string solution(int n, int k, vector<string> cmd) {
    string answer(n,'X');

    set<int> table;
    stack<int> s;
    for(int i=0;i<n;i++) table.insert(i);

    auto cur = table.find(k);

    for(int i = 0; i < cmd.size(); i++){
        if(cmd[i][0]=='U'){
            int x = stoi(cmd[i].substr(2));
            while(x--) {
                cur = prev(cur);
            }
        }else if(cmd[i][0]=='D'){
            int x = stoi(cmd[i].substr(2));
            while(x--) {
                cur = next(cur);
            }
        }else if(cmd[i][0]=='C'){
            s.push(*cur);
            cur = table.erase(cur);
            if(cur==table.end()) {
                cur = prev(cur);
            }
        }else{
            table.insert(s.top());
            s.pop();
        }
    }

    for(int i:table) answer[i]='O';

    return answer;
}
```

## 연결리스트 이용한 풀이
```cpp
#include <string>
#include <vector>
#include <stack>
using namespace std;

int cur;
stack<int> s;

struct Node {
    int num;
    Node* prev;
    Node* next;
    Node(int num):num(num),prev(NULL),next(NULL){};
};

vector<Node*> v;

string solution(int n, int k, vector<string> cmd) {
    string answer(n,'X');
    
    // 연결리스트 생성 및 연결
    v = vector<Node*>(n);
    
    for(int i=0;i<n;i++)
        v[i] = new Node(i);
    
    v[0]->next = v[1];
    v[n-1]->prev = v[n-2];
    
    for(int i=1;i<n-1;i++){
        v[i]->next = v[i+1];
        v[i]->prev = v[i-1];
    }
    
    // cmd 수행
    cur = k;
    for(int i = 0; i < cmd.size(); i++){
        if(cmd[i][0]=='D'){
            int x = stoi(cmd[i].substr(2));
            while(x--) {
                cur = v[cur]->next->num;
            }
            
        }else if( cmd[i][0]=='U'){
            int x = stoi(cmd[i].substr(2));
            while(x--) {
                cur = v[cur]->prev->num;
            }
        }else if(cmd[i][0]=='C'){
            s.push(cur);
            if(v[cur]->prev != NULL) {
                v[cur]->prev->next = v[cur]->next;
            }
            if(v[cur]->next != NULL){
                v[cur]->next->prev = v[cur]->prev;
                cur = v[cur]->next->num;
            }else {
                cur = v[cur]->prev->num;
            }
        }else{
            int r = s.top(); 
            s.pop();
            if(v[r]->prev != NULL) {
                v[r]->prev->next = v[r];
            }
            if(v[r]->next != NULL) {
                v[r]->next->prev = v[r];
            }
        }
    }
    
    int leftCheck, rightCheck;
    leftCheck = rightCheck = cur;
    
    answer[cur] = 'O';
    
    while(v[leftCheck]->prev != NULL){
        leftCheck = v[leftCheck]->prev->num;
        answer[leftCheck] = 'O';
    }
    
    while(v[rightCheck]->next != NULL){
        rightCheck = v[rightCheck]->next->num;
        answer[rightCheck] = 'O';
    }

    return answer;
}
```

## Comments
효율성 테스트를 통과하지 못해서 풀이를 찾아보았다.  
[이 블로그](https://yjyoon-dev.github.io/kakao/2021/07/24/kakao-tableedit/) 를 참고하였는데 풀이가 정말 좋다.
vector.erase()가 시간이 많이 걸려 리스트를 사용할 생각은 했었다!  
하지만 직접 Node 구조체를 만든 후, vector에 넣어줘서 인덱스 접근까지 같이 활용해주는게 정말 좋은 방법인 거 같다.  
그리고 set을 이용한 풀이 법도 너무 깔끔하다.   
set은 자동으로 정렬이 되는 것도 포인트이고, std::prev, std::next 활용법도 배울 수 있었다. 

## STL list를 이용한 풀이 (효율성 테스트 FAIL, 시간 초과)
```cpp
#include <string>
#include <vector>
#include <stack>
#include <cstdlib>
#include <list>
#include <iostream>

using namespace std;

string solution(int n, int k, vector<string> cmd) {
    string answer = "";
    list<int> table;
    for(int i = 0; i < n; i++) {
        table.push_back(i);
        answer += "X";
    }
    stack<int> s;
    for(int i = 0; i < cmd.size(); i++) {
        char command = cmd[i][0];
        if(command == 'U') {
            k -= stoi(cmd[i].substr(2));
        }else if(command == 'D') {
            k += stoi(cmd[i].substr(2));
        }else if(command == 'C'){
            auto iter = table.begin();
            for(int j = 0; j < k; j++) {
                iter++;
            }
            s.push(*iter);
            table.erase(iter);
            if(table.size() == k) {
                k -= 1;
            }
        }else{
            int top = s.top();
            s.pop();
            auto iter = table.begin();
            int idx = 0;
            while(iter != table.end() && top > *iter) {
                iter++;
                idx++;
            }
            if(idx <= k) {
                k += 1;
            }
            table.insert(iter, top);
        }
    }
    for(auto iter = table.begin(); iter != table.end(); iter++) {
        answer[*iter] = 'O';
    }
    return answer;
}
```

## 효율성 통과하지 못한 풀이 (시간 초과)
```cpp
#include <string>
#include <vector>
#include <stack>
#include <cstdlib>
#include <iostream>

using namespace std;

string solution(int n, int k, vector<string> cmd) {
    string answer = "";
    vector<int> table;
    table.resize(n);
    for(int i = 0; i < n; i++) {
        table[i] = i;
        answer += "X";
    }
    stack<pair<int,int>> s;
    for(int i = 0; i < cmd.size(); i++) {
        char command = cmd[i][0];
        if(command == 'U') {
            k -= stoi(cmd[i].substr(2));
        }else if(command == 'D') {
            k += stoi(cmd[i].substr(2));
        }else if(command == 'C'){
            s.push({k, table[k]});
            table.erase(table.begin() + k);
            if(table.size() == k) {
                k -= 1;
            }
        }else{
            pair<int,int> top = s.top();
            s.pop();
            if(top.first <= k) {
                k += 1;
            }
            table.insert(table.begin() + top.first, top.second);
        }
    }
    for(int i = 0; i < table.size(); i++) {
        answer[table[i]] = 'O';
    }
    return answer;
}
```