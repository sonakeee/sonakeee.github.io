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

title: "[SWEA] Ladder2"
excerpt: "[SWEA] Ladder2"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, python, SWEA, 구현]
date: 2021-08-14
last_modified_at: 2021-08-14
---
# [SWEA] Ladder2

Problem URL : [Ladder2](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14BgD6AEECFAYh&categoryId=AV14BgD6AEECFAYh&categoryType=CODE&problemTitle=ladder&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1)

```python
for tc in range(1, 11):
    tc = int(input())
    arr = [list(map(int, input().split())) for _ in range(100)]
    minCost = 1e9
    ans = 0
    for i in range(100):
        if arr[0][i] == 1:
            idx = i
            cnt = 0
            for j in range(100):
                if idx > 0 and arr[j][idx - 1] == 1:
                    while idx > 0 and arr[j][idx - 1] > 0:
                        idx -= 1
                        cnt += 1
                elif idx < 99 and arr[j][idx + 1] == 1:
                    while idx < 99 and arr[j][idx + 1] > 0:
                        idx += 1
                        cnt += 1
            if minCost >= cnt:
                minCost = cnt
                ans = i

    print('#{} {}'.format(tc, ans))
```