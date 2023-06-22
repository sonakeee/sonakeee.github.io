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

title: "[Softeer] Garage game"
excerpt: "[Softeer] Garage game"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Softeer, DFS, BFS]
date: 2021-09-12
last_modified_at: 2021-09-12
---
# [Softeer] Garage game

Problem URL : [Garage game](https://softeer.ai/practice/info.do?eventIdx=1&psProblemId=540)

## 시간 초과 풀이(N <= 4일 때만 통과)

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;
int ans = 0;
int n;
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};

void dfs(vector<vector<int>> map, int sum, int cnt) {
    if (cnt == 3) {
        if (ans < sum) {
            ans = sum;
        }
        return;
    }
    for (int i = 0; i < n; i++) {
        for (int j = n - 1; j >= 0; j--) {
            if (map[i][j] == 0) {
                map[i].erase(map[i].begin() + j);
            }
        }
    }
    vector<vector<bool>> visit(n, vector<bool>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (!visit[i][j]) {
                vector<vector<int>> tmp = map;
                queue<pair<int, int>> q;
                int point = 0;
                int value = tmp[i][j];
                tmp[i][j] = 0;
                q.push({i, j});
                int minX = i;
                int maxX = i;
                int minY = j;
                int maxY = j;
                while (!q.empty()) {
                    int x = q.front().first;
                    int y = q.front().second;
                    q.pop();
                    if (visit[x][y]) continue;
                    visit[x][y] = true;
                    tmp[x][y] = 0;
                    point++;
                    if (minX > x) minX = x;
                    if (maxX < x) maxX = x;
                    if (minY > y) minY = y;
                    if (maxY < y) maxY = y;
                    for (int dir = 0; dir < 4; dir++) {
                        int nx = x + dx[dir];
                        int ny = y + dy[dir];
                        if (nx < 0 || ny < 0 || nx >= n || ny >= n || visit[nx][ny] || tmp[nx][ny] != value) continue;
                        q.push({nx, ny});
                    }
                }
                point += (maxX - minX + 1) * (maxY - minY + 1);
                dfs(tmp, sum + point, cnt + 1);
            }
        }
    }
}

int main(int argc, char** argv) {
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    cin >> n;
    vector<vector<int>> map(n, vector<int>(n * 3));
    for (int i = 3 * n - 1; i >= 0; i--) {
        for (int j = 0; j < n; j++) {
            cin >> map[j][i];
        }
    }
    dfs(map, 0, 0);
    cout << ans << endl;
    return 0;
}
```
## 시간초과 풀이2

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm></algorithm>

using namespace std;
int ans = 0;
int n;
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};



void dfs(vector<vector<int>> &map, int sum, int cnt) {
    if (cnt == 3) {
        if (ans < sum) {
            ans = sum;
        }
        return;
    }

    vector<vector<bool>> visit(n, vector<bool>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (!visit[i][j]) {
                queue<pair<int, int>> q;
                vector<pair<int, int>> tmp;
                int point = 0;
                int value = map[i][j];
                map[i][j] = 0;
                q.push({i, j});
                int minX = i;
                int maxX = i;
                int minY = j;
                int maxY = j;
                while (!q.empty()) {
                    int x = q.front().first;
                    int y = q.front().second;
                    q.pop();
                    if (visit[x][y]) continue;
                    visit[x][y] = true;
                    point++;
                    tmp.push_back({x, y});
                    if (minX > x) minX = x;
                    if (maxX < x) maxX = x;
                    if (minY > y) minY = y;
                    if (maxY < y) maxY = y;
                    for (int dir = 0; dir < 4; dir++) {
                        int nx = x + dx[dir];
                        int ny = y + dy[dir];
                        if (nx < 0 || ny < 0 || nx >= n || ny >= n || visit[nx][ny] || map[nx][ny] != value) continue;
                        q.push({nx, ny});
                    }
                }
                point += (maxX - minX + 1) * (maxY - minY + 1);
                sort(tmp.begin(), tmp.end(), greater<>());
                for(int k = 0; k < tmp.size(); k++) {
                    map[tmp[k].first].erase(map[tmp[k].first].begin() + tmp[k].second);
                }
                dfs(map, sum + point, cnt + 1);
                for(int k = tmp.size() - 1; k >= 0; k--) {
                    map[tmp[k].first].insert(map[tmp[k].first].begin() + tmp[k].second, value);
                }
            }
        }
    }
}

int main(int argc, char** argv) {
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    cin >> n;
    vector<vector<int>> map(n, vector<int>(n * 3));
    for (int i = 3 * n - 1; i >= 0; i--) {
        for (int j = 0; j < n; j++) {
            cin >> map[j][i];
        }
    }
    dfs(map, 0, 0);
    cout << ans << endl;
    return 0;
}
```

## 시간초과 풀이3
```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm></algorithm>

using namespace std;
int ans = 0;
int n;
int dx[4] = {1, -1, 0, 0};
int dy[4] = {0, 0, 1, -1};



void dfs(vector<vector<int>> &map, int sum, int cnt) {
    if (cnt == 3) {
        if (ans < sum) {
            ans = sum;
        }
        return;
    }

    vector<vector<bool>> visit(n, vector<bool>(n));
    
    for (int i = 0; i < n; i--) {
        for (int j = 0; j < n; j--) {
            if (!visit[i][j]) {
                queue<pair<int, int>> q;
                vector<pair<int, int>> tmp;
                int point = 0;
                int value = map[i][j];
                map[i][j] = 0;
                q.push({i, j});
                int minX = i;
                int maxX = i;
                int minY = j;
                int maxY = j;
                while (!q.empty()) {
                    int x = q.front().first;
                    int y = q.front().second;
                    q.pop();
                    if (visit[x][y]) continue;
                    visit[x][y] = true;
                    point++;
                    tmp.push_back({x, y});
                    if (minX > x) minX = x;
                    if (maxX < x) maxX = x;
                    if (minY > y) minY = y;
                    if (maxY < y) maxY = y;
                    for (int dir = 0; dir < 4; dir++) {
                        int nx = x + dx[dir];
                        int ny = y + dy[dir];
                        if (nx < 0 || ny < 0 || nx >= n || ny >= n || visit[nx][ny] || map[nx][ny] != value) continue;
                        q.push({nx, ny});
                    }
                }
                point += (maxX - minX + 1) * (maxY - minY + 1);
                sort(tmp.begin(), tmp.end(), greater<>());
                for(int k = 0; k < tmp.size(); k++) {
                    map[tmp[k].first].erase(map[tmp[k].first].begin() + tmp[k].second);
                }
                dfs(map, sum + point, cnt + 1);
                for(int k = tmp.size() - 1; k >= 0; k--) {
                    map[tmp[k].first].insert(map[tmp[k].first].begin() + tmp[k].second, value);
                }
            }
        }
    }
}

int main(int argc, char** argv) {
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    cin >> n;
    vector<vector<int>> map(n, vector<int>(n * 3));
    for (int i = 3 * n - 1; i >= 0; i--) {
        for (int j = 0; j < n; j++) {
            cin >> map[j][i];
        }
    }
    dfs(map, 0, 0);
    cout << ans << endl;
    return 0;
}
```
