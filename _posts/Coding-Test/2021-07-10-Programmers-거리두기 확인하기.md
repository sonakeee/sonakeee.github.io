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

title: "[Programmers] 거리두기 확인하기"
excerpt: "[Programmers] 거리두기 확인하기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-10
last_modified_at: 2021-07-10
---
# [Programmers] 거리두기 확인하기

Problem URL : [거리두기 확인하기](https://programmers.co.kr/learn/courses/30/lessons/81302?language=cpp)

## 내 풀이

```cpp
#include <string>
#include <vector>
#include <cstring>
#include <queue>
#define p pair<int,int>

using namespace std;

bool outRange(int x, int y) {
    if (x < 0 || x >= 5 || y < 0 || y >= 5) {
        return true;
    }
    return false;
}

vector<int> solution(vector<vector<string>> places) {
    int dx[4] = { 1, -1, 0 , 0 };
    int dy[4] = { 0, 0, 1 , -1 };

    vector<int> answer;
    char map[5][5];
    bool visit[5][5];
    vector<p> people;
    bool covid;
    for (int i = 0; i < places.size(); i++) {
        covid = false;
        people.clear();
        for (int j = 0; j < 5; j++) {
            for (int k = 0; k < 5; k++) {
                map[j][k] = places[i][j][k];
                if (map[j][k] == 'P') {
                    people.push_back({ j,k });
                }
            }
        }

        for (int j = 0; j < people.size(); j++) {
            int r = people[j].first;
            int c = people[j].second;
            queue<pair<p, int>> q;
            memset(visit, 0, sizeof(visit));
            q.push({ {r,c},0 });
            while (!q.empty()) {
                int x = q.front().first.first;
                int y = q.front().first.second;
                int dist = q.front().second;
                q.pop();
                visit[x][y] = true;
                for (int k = 0; k < 4; k++) {
                    int nx = x + dx[k];
                    int ny = y + dy[k];
                    if (outRange(nx, ny) || visit[nx][ny] || map[nx][ny] == 'X') {
                        continue;
                    }
                    if (map[nx][ny] == 'P' && dist <= 1) {
                        covid = true;
                        break;
                    }
                    q.push({ { nx,ny }, dist + 1 });
                }
                if (covid) {
                    break;
                }
            }
            if (covid) {
                break;
            }
        }

        if (covid) {
            answer.push_back(0);
        } else {
            answer.push_back(1);
        }
    }
    return answer;
}
```

## 모범 답안

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

bool is_valid_place(const vector<string>& place)
{
    constexpr size_t N = 5;

    vector<vector<int>> is_in_use(
        N,
        vector<int>(N, false)
    );

    int di[] = {1,-1,0,0};
    int dj[] = {0,0,1,-1};

    for(size_t i=0; i!=N; ++i)
        for(size_t j=0; j!=N; ++j)
            if(place[i][j] == 'P'){
                for(size_t k=0; k!=4; ++k){
                    size_t moved_i = i + di[k];
                    size_t moved_j = j + dj[k];

                    if(moved_i >= N || moved_j >= N)
                        continue;

                    switch(place[moved_i][moved_j]){
                        case 'P':
                            return false;
                        case 'O':
                            if(is_in_use[moved_i][moved_j])
                                return false;

                            is_in_use[moved_i][moved_j] = true;
                            break;
                        case 'X':
                            break;
                    }
                }
            }

    return true;
}

vector<int> solution(vector<vector<string>> places) {
    vector<int> answer(5);
    for(size_t i=0; i!=5; ++i)
        answer[i] = is_valid_place(places[i]);
    return answer;
}
```

## Comments
모범답안은 프로그래머스에서 fienestar님이 올려주신 풀이다.  
정말 깔끔하다!