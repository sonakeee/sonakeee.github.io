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

title: "[Programmers] 자물쇠와 열쇠"
excerpt: "[Programmers] 자물쇠와 열쇠"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 구현]
date: 2021-08-20
last_modified_at: 2021-08-20
---
# [Programmers] 자물쇠와 열쇠

Problem URL : [자물쇠와 열쇠](https://programmers.co.kr/learn/courses/30/lessons/60059)

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> rotate(vector<vector<int>> &key) {
  int M = key.size();
  vector<vector<int>> temp(M,vector<int>(M));

  for(int i = 0; i < M; i++)
    for(int j = 0; j < M; j++)
      temp[i][j] = key[j][M - i - 1];

  for(int i = 0; i < M; i++)
    for(int j = 0; j < M; j++)
        key[i][j] = temp[i][j];

	return key;
}

bool check(int x, int y, vector<vector<int>> &key, vector<vector<int>> board) {
    int M = key.size(); 
    int B = board.size();
	for (int i = 0; i < M; i++) {
		for (int j = 0; j < M; j++) {
			board[x + i][y + j] += key[i][j];
		}
	}

	for (int i = M - 1; i < B - M + 1; i++) {
		for (int j = M - 1; j < B - M + 1; j++) {
			if (board[i][j] != 1) { // 돌기 + 홈 = 1 이어야 한다.
				return false;
			}
		}
	}

	return true;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {
	bool answer = false;

	int M = key.size();
	int N = lock.size();
	
	vector<vector<int>>board(N + 2* M - 2,vector<int>(N + 2* M - 2,0));
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			board[i + M - 1][j + M - 1] = lock[i][j];
		}
	}
	
	for (int l = 0; l < 4; l++) {
		for (int i = 0; i < M + N - 1; i++) {
			for (int j = 0; j < M + N - 1; j++) {
				if (check(i, j, key, board)) {
					answer = true;
					return true;
				}
			}
		}
		key = rotate(key);
	}
	return answer;
}
```
