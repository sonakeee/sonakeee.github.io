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

title: "[BOJ] Cubeditor"
excerpt: "[BOJ] Cubeditor"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, BOJ, 문자열]
date: 2021-06-01
last_modified_at: 2021-06-01
---
# [BOJ] Cubeditor

Problem URL : [Cubeditor](https://www.acmicpc.net/problem/1701)

```cpp
#include <iostream>
#include <algorithm>
#include <set>
#include <string>
using namespace std;

int ans, cnt;
string word;
set<string> strings;

int main() {
	ios_base::sync_with_stdio(false), cin.tie(NULL), cout.tie(NULL);
	cin >> word;
	int length = word.length();
	for (int i = 0; i < length; ++i) {
		strings.insert(word);
		word = word.substr(1);
	}
	string pre = "";
	for (const auto &str : strings) {
		cnt = 0;
		while (pre[cnt] == str[cnt]) ++cnt;
		ans = max(ans, cnt);
		pre = str;
	}
	cout << ans;

	return 0;
}
```