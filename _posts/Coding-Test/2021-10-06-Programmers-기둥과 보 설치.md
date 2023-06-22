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

title: "[Programmers] 기둥과 보 설치"
excerpt: "[Programmers] 기둥과 보 설치"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 구현, 비트마스크]
date: 2021-10-06
last_modified_at: 2021-10-07
---
# [Programmers] 기둥과 보 설치

Problem URL : [기둥과 보 설치](https://programmers.co.kr/learn/courses/30/lessons/60061)

```cpp
#include <string>
#include <vector>
#include <cstring>

using namespace std;

vector<vector<int>> solution(int n, vector<vector<int>> build_frame) {
    vector<vector<int>> answer;
    int visit[102][102];
    memset(visit, 0, sizeof(visit));
    
    for(int i = 0; i < build_frame.size(); i++) {
        int x = build_frame[i][0];
        int y = build_frame[i][1];
        int a = build_frame[i][2];
        int b = build_frame[i][3];
        if(b == 1) {
            if(a == 0) {
                // 기둥 설치 조건
                // 바닥이거나, 좌표에 위방향 기둥말고 하나라도 설치가 되어있어야 한다.
                if(y == 0 || (visit[x][y] & 14)) {
                    visit[x][y] |= 1;
                    visit[x][y+1] |= 2;
                }
            }else {
                // 보 설치 조건
                // 좌우에 밑으로 기둥이 설치되어있거나
                // 양쪽에 보가 있어야 한다.
                if(visit[x][y] & 2 || ((visit[x][y] & 8) && (visit[x+1][y] & 4)) || visit[x+1][y] & 2) {
                    visit[x][y] |= 4;
                    visit[x+1][y] |= 8;
                }
            }
        }else {
            if(a == 0){
                // 기둥 삭제 불가 조건
                // 위에 좌표에 아래, 위방향 기둥만 있을 때
                if(visit[x][y+1] == 3) continue;
                // 왼쪽에 보가 있고, 왼쪽보의 왼쪽밑에 기둥이 없으면서
                // 좌우로 보가 연결되어있지 않을 때
                if((visit[x][y+1] & 8) && !(visit[x-1][y+1] & 2)) {
                    if(!(visit[x-1][y+1] & 8) || !(visit[x][y+1] & 4)) continue;
                }
                // 오른쪽에 보가 있고, 오른쪽보의 오른쪽밑에 기둥이 없으면서
                // 좌우로 보가 연결되어있지 않을 때
                if((visit[x][y+1] & 4) && !(visit[x+1][y+1] & 2)) {
                    if(!(visit[x][y+1] & 8) || !(visit[x+1][y+1] & 4)) continue;
                }
                visit[x][y] &= 14;
                visit[x][y+1] &= 13;
            }else {
                // 보 삭제 불가 조건
                // 왼쪽 보가 존재하고 왼쪽 보의 좌우 밑 기둥이 없을 때
                if((visit[x][y] & 8) && !(visit[x-1][y] & 2) && !(visit[x][y] & 2)) continue;
                // 오른쪽 보가 존재하고 오른쪽 보의 좌우 밑 기둥이 없을 때
                if((visit[x+1][y] & 4) && !(visit[x+1][y] & 2) && !(visit[x+2][y] & 2)) continue;
                // 받치고 있던 기둥이 있고 삭제할 보에게만 의지하고 있을 때
                if(visit[x][y] == 5 || visit[x+1][y] == 9) continue;
                visit[x][y] &= 11;
                visit[x+1][y] &= 7;
            }
        }
    }
    for(int x = 0; x <= n; x++) {
        for(int y = 0; y <= n; y++) {
            if(visit[x][y] & 1){
                answer.push_back({x,y,0});
            }
            if(visit[x][y] & 4){
                answer.push_back({x,y,1});
            }
        }    
    }
    return answer;
}
```
## Comments
비트마스크를 써서 좌표마다 기둥과 보 설치 정보를 나타내주었다.  
(1: 위로 기둥, 2: 아래로 기둥, 4: 오른쪽으로 보, 8: 왼쪽으로 보)  
삭제 구현이 상당히 까다로웠다...  

그리고 크게 두 가지 실수를 해서 삽질을 했었다.  
[1] 연산자 우선 순위를 착각했는데, 비트연산자보다 비교연산자가 우선순위가 높은 것을 기억하자.    [2] visit에 쓰레기값(?)이 들어있어서 테케가 실패했었다. memset을 습관화 하도록 해야겠다.