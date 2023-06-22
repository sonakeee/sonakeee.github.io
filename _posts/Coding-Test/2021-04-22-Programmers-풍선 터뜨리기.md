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

title: "[Programmers] 풍선 터뜨리기"
excerpt: "[Programmers] 풍선 터뜨리기"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, Programmers, 동적 프로그래밍]
date: 2021-04-19
last_modified_at: 2021-04-19
---
# [Programmers] 풍선 터뜨리기

Problem URL : [풍선 터뜨리기](https://programmers.co.kr/learn/courses/30/lessons/68646)

```cpp
#include <string>
#include <vector>

using namespace std;

int dpLeft[1000000], dpRight[1000000];

int solution(vector<int> a) {
	int answer = 0;
	dpLeft[0] = a[0];
	// 처음 풍선부터 i 풍선까지의 최솟값을 기록
	for (int i = 1; i < a.size(); i++)
		dpLeft[i] = dpLeft[i - 1] > a[i] ? a[i] : dpLeft[i - 1];
	
	int size = a.size();
	dpRight[size - 1] = a[size - 1];
	// 마지막 풍선부터 i풍선까지의 최솟값을 기록
	for (int i = size - 2; i >= 0; i--) 
		dpRight[i] = dpRight[i + 1] > a[i] ? a[i] : dpRight[i + 1];

	// 왼쪽, 오른쪽 중에 한 방향으로 풍선을 터뜨릴 수 있는지 체크
	for (int i = 0; i < size; i++)
		if (a[i] == dpLeft[i] || a[i] == dpRight[i]) answer++;
	
	return answer;
}
```
## Comments  
***
처음에는 세그먼트 트리를 이용해서 부분 최솟값을 구할 생각을 했다.  
생각해보니 한 끝이 고정된 (처음과 마지막 풍선) 부분의 최솟값이어서 dp 배열로 해결된다.