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

title: "[Programmers] 방금그곡"
excerpt: "[Programmers] 방금그곡"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 문자열]
date: 2021-07-05
last_modified_at: 2021-07-05
---
# [Programmers] 방금그곡

Problem URL : [방금그곡](https://programmers.co.kr/learn/courses/30/lessons/17683)


```cpp
#include <string>
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

unordered_map<string,string> um;

bool cmp(pair<string, int> a, pair<string, int> b) {
    return a.second > b.second;
}

string convert(string s) {
    string ret = "";
    for (int i = 0; i < s.size(); i++) {
        if (s[i + 1] == '#') {
            ret += um[s.substr(i, 2)];
            i++;
        } else {
            ret += s[i];
        }
    }
    return ret;
}

string solution(string m, vector<string> musicinfos) {
    string answer = "";
    um["C#"] = "c";
    um["D#"] = "d";
    um["F#"] = "f";
    um["G#"] = "g";
    um["A#"] = "a";

    vector<pair<string, int>> ansList;
    m = convert(m);
    int listenLength = m.size();

    for (int i = 0; i < musicinfos.size(); i++) {
        string info = musicinfos[i];

        vector<string> infos;
        string token = "";
        for (int j = 0; j < info.size(); j++) {
            if (info[j] == ',') {
                infos.push_back(token);
                token = "";
            } else {
                token += info[j];
            }
        }
        infos.push_back(token);

        int startTime = stoi(infos[0].substr(0, 2)) * 60 + stoi(infos[0].substr(3, 2));
        int endTime = stoi(infos[1].substr(0, 2)) * 60 + stoi(infos[1].substr(3, 2));
        int time = endTime - startTime;

        infos[3] = convert(infos[3]);
        int musicLength = infos[3].size();

        if (time > musicLength) {
            string tmp = "";
            for (int j = 0; j < time; j++) {
                tmp += infos[3][j % musicLength];
            }
            infos[3] = tmp;
        } else {
            infos[3] = infos[3].substr(0, time);
        }

        musicLength = infos[3].size();
        
        if (listenLength > musicLength) {
            continue;
        } else {
            if (infos[3].find(m) != string::npos) {
                ansList.push_back({infos[2], time});
            }
        }
    }

    if (ansList.empty()) {
        return "(None)";
    }


    stable_sort(ansList.begin(), ansList.end(), cmp);
    answer = ansList[0].first;
    return answer;
}
```