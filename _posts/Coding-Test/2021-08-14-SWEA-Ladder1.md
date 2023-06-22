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

title: "[SWEA] Ladder1"
excerpt: "[SWEA] Ladder1"
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
# [SWEA] Ladder1

Problem URL : [Ladder1](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14ABYKADACFAYh&categoryId=AV14ABYKADACFAYh&categoryType=CODE&problemTitle=ladder&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1)

```python
for tc in range(1, 11):
    tc = int(input())
    arr = [list(map(int, input().split())) for _ in range(100)]
    idx = arr[99].index(2)
    for i in range(99,-1,-1):
        if idx > 0 and arr[i][idx - 1] == 1:
            while idx > 0 and arr[i][idx - 1] > 0:
                idx -= 1
        elif idx < 99 and arr[i][idx + 1] == 1:
            while idx < 99 and arr[i][idx + 1] > 0:
                idx += 1
    print('#{} {}'.format(tc, idx))
```