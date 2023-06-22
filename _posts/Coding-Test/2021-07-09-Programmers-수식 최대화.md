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

title: "[Programmers] 수식 최대화"
excerpt: "[Programmers] 수식 최대화"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-09
last_modified_at: 2021-07-09
---
# [Programmers] 수식 최대화

Problem URL : [수식 최대화](https://programmers.co.kr/learn/courses/30/lessons/67257)


```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <set>
using namespace std;

long long calc(long long a, long long b, char opr) {
    if (opr == '+')
        return a + b;
    else if (opr == '-')
        return a - b;
    else if (opr == '*')
        return a * b;
}

long long solution(string expression) {
    vector <long long> nums;
    vector <char> operators;
    set<char> operator_set;
    string n = "";
    for (int i = 0; i < expression.length(); i++) {
        if (expression[i] == '*' || expression[i] == '+' || expression[i] == '-') {
            nums.push_back(stoi(n));
            n = "";
            operators.push_back(expression[i]);
            operator_set.insert(expression[i]);
        } else {
            n += expression[i];
        }
    }
    nums.push_back(stoi(n));
    // set 은 중복 없이 자동 정렬
    vector<char>operator_list(operator_set.begin(), operator_set.end()); 
    long long max_result = 0;
    do {
        vector <long long>  tmp_nums = nums;
        vector <char> tmp_operators = operators;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < tmp_operators.size(); j++) {
                if (tmp_operators[j] == operator_list[i]) {
                    tmp_nums[j] = calc(tmp_nums[j], tmp_nums[j + 1], operator_list[i]);
                    tmp_nums.erase(tmp_nums.begin() + j + 1);
                    tmp_operators.erase(tmp_operators.begin() + j);
                    j--;
                }
            }
        }
        if (abs(tmp_nums.front()) > max_result)
            max_result = abs(tmp_nums.front());
    } while (next_permutation(operator_list.begin(), operator_list.end()));
    return max_result;
}
```