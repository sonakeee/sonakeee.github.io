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

title: "[BOJ] 광고 삽입"
excerpt: "[BOJ] 광고 삽입"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 누적 합]
date: 2021-09-27
last_modified_at: 2021-09-27
---
# [BOJ] 광고 삽입

Problem URL : [광고 삽입](https://programmers.co.kr/learn/courses/30/lessons/72414)

```cpp
#include <string>
#include <vector>

using namespace std;

typedef long long ll;
ll v[360001];

int toSec(string t) {
    int ret = 0;
    ret += ((t[0] - '0') * 10 + (t[1] - '0')) * 3600;
    ret += ((t[3] - '0') * 10 + (t[4] - '0')) * 60;
    ret += (t[6] - '0') * 10 + t[7] - '0';
    return ret;
}

string toString(int t) {
    string hour = to_string(t / 3600);
    string min = to_string((t % 3600) / 60);
    string sec = to_string(t % 60);
    if (hour.size() == 1) hour = '0' + hour;
    if (min.size() == 1) min = '0' + min;
    if (sec.size() == 1) sec = '0' + sec;
    return hour + ':' + min + ':' + sec;
}

string solution(string play_time, string adv_time, vector<string> logs) {
    int n = toSec(play_time);
    int interval = toSec(adv_time);
    for (string &log : logs) {
        int s = toSec(log.substr(0, 8));
        int e = toSec(log.substr(9));
        v[s]++;
        v[e]--;
    }

    for (int i = 1; i <= n; i++) v[i] += v[i - 1];
    for (int i = 1; i <= n; i++) v[i] += v[i - 1];

    int insertTime = 0;
    ll sum = v[interval - 1];
    ll maxSum = sum;
    
    for (int s = 1; s + interval - 1 <= n; s++) {
        sum = v[s + interval - 1] - v[s - 1];
        if (maxSum < sum) {
            maxSum = sum;
            insertTime = s;
        }
    }
    return toString(insertTime);
}
```
## Comments
누적 합 개념을 잘 배워두자.  
올해 카카오, 네이버 클라우드 코테에서 알아뒀어야 하는 스킬이었는데 이제서야 배웠다 ㅠㅠ