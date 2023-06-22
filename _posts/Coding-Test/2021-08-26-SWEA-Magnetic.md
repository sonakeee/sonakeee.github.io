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

title: "[SWEA] Magnetic"
excerpt: "[SWEA] Magnetic"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, python, SWEA, QUEUE]
date: 2021-08-26
last_modified_at: 2021-08-26
---
# [SWEA] Magnetic

Problem URL : [Magnetic](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14hwZqABsCFAYD)

```python
for tc in range(1, 11):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    ans = 0
    for i in range(N):
        q = []
        for j in range(N):
            if not q:
                if arr[j][i] == 1:  # 비어 있을 때는 N극을 추가
                    q.append(1)
            elif q[-1] + arr[j][i] == 3:  # 다른 극일 때 큐에 추가
                q.append(arr[j][i])
        ans += len(q)//2  # N극, S극 2개씩 뭉텅이 하나에 교착상태 1개

    print('#{} {}'.format(tc, ans))
```

