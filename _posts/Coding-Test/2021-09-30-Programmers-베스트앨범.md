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

title: "[BOJ] 베스트앨범"
excerpt: "[BOJ] 베스트앨범"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 해시]
date: 2021-09-30
last_modified_at: 2021-09-30
---
# [BOJ] 베스트앨범

Problem URL : [베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579)

```cpp
#include <string>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

bool cmp(const pair<string,int>& a, const pair<string,int>& b) {
	return a.second > b.second;
}

void pickMusic(vector<int> &plays, vector<int> &answer, vector<int> &musics) {
    int first = -1, second = -1, firstIdx, secondIdx;
    for(int i = 0; i < musics.size(); i++) {
        if(first < plays[musics[i]]) {
            second = first;
            secondIdx = firstIdx;
            first = plays[musics[i]];
            firstIdx = musics[i];
        }else {
            if(second < plays[musics[i]]) {
                second = plays[musics[i]];
                secondIdx = musics[i];
            }
        }
    }
    answer.push_back(firstIdx);
    if(second != -1) {
        answer.push_back(secondIdx);
    }
}

vector<int> solution(vector<string> genres, vector<int> plays) {
    vector<int> answer;
    map<string, int> music;
    map<string, vector<int>> musicList;
    for (int i = 0; i < genres.size(); i++) {
        music[genres[i]] += plays[i];
        musicList[genres[i]].push_back(i);
    }
    vector<pair<string, int>> vec(music.begin(), music.end());  // [1]
    sort(vec.begin(), vec.end(), cmp);
    
    for(int i = 0; i < vec.size(); i++) {
        pickMusic(plays, answer, musicList[vec[i].first]);
    }
    return answer;
}
```
## Comments
처음에는 장르 2개만 고르는 걸로 이해해서 틀렸었다...  
map을 vector로 바꾸는 [1] 방식은 기억해두자