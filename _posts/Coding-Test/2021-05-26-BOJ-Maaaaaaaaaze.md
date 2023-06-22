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

title: "[BOJ] Maaaaaaaaaze"
excerpt: "[BOJ] Maaaaaaaaaze"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, BOJ, BFS, DFS]
date: 2021-05-26
last_modified_at: 2021-05-26
---
# [BOJ] Maaaaaaaaaze

Problem URL : [Maaaaaaaaaze](https://www.acmicpc.net/problem/16985)

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;

struct maze {
    int x, y, z;
};

int original[5][5][5], board[5][5][5], order[5];
int dist[5][5][5], ans = 1e9;
const int dx[] = { 1, -1, 0, 0, 0, 0 }, dy[] = { 0, 0, 1, -1, 0, 0 }, dz[] = { 0 ,0, 0, 0, 1, -1 };

void bfs() {
    if (!board[0][0][0] || !board[4][4][4]) return;
    queue<maze> q;
    q.push({ 0, 0, 0 });
    memset(dist, -1, sizeof(dist));
    dist[0][0][0] = 0;
    while (!q.empty()) {
        int x = q.front().x, y = q.front().y, z = q.front().z; q.pop();
        if (x == 4 && y == 4 && z == 4) {
            ans = min(ans, dist[x][y][z]);
            if (ans == 12) {
                printf("12\n");
                exit(0);
            }
            return;
        }
        for (int i = 0; i < 6; i++) {
            int nx = x + dx[i], ny = y + dy[i], nz = z + dz[i];
            if (nx < 0 || nx >= 5 || ny < 0 || ny >= 5 || nz < 0 || nz >= 5) continue;
            if (board[nx][ny][nz] && dist[nx][ny][nz] == -1) {
                q.push({ nx, ny, nz });
                dist[nx][ny][nz] = dist[x][y][z] + 1;
            }
        }
    }
}

void rotate(int s) {
    int tmp[5][5];
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            tmp[j][4 - i] = board[s][i][j];
        }
    }
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            board[s][i][j] = tmp[i][j];
        }
    }
}

void dfs(int idx) {
    if (idx == 5) {
        bfs();
        return;
    }
    for (int i = 0; i < 4; i++) {
        rotate(idx);
        dfs(idx + 1);
    }
}

int main() {
    ios_base::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            for (int k = 0; k < 5; k++) {
                cin >> original[i][j][k];
            }
        }
    }
    for (int i = 0; i < 5; i++) order[i] = i;
    do {
        for (int i = 0; i < 5; i++) {
            memcpy(board[order[i]], original[i], sizeof(original[i]));
        }
        dfs(0);
    } while (next_permutation(order, order + 5));
    ans = ans == 1e9 ? -1 : ans;
    cout << ans;
    return 0;
}
```