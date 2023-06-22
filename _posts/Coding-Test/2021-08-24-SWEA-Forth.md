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

title: "[SWEA] Forth"
excerpt: "[SWEA] Forth"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test
tags:
  - [Coding Test, python, SWEA, STACK]
date: 2021-08-24
last_modified_at: 2021-08-24
---
# [SWEA] Forth

Problem URL : [Forth](https://swexpertacademy.com/main/learn/course/subjectDetail.do?courseId=AVuPDN86AAXw5UW6&subjectId=AWOVIc7KqfQDFAWg)

```python
TC = int(input())
for tc in range(1, TC + 1):
    exp = list(input().split())
    stack = []
    error = False
    for i in range(len(exp)):
        if exp[i].isdigit():
            stack.append(exp[i])
        else:
            if exp[i] == '.':
                if len(stack) != 1:  # [1]
                    error = True
                break

            if len(stack) < 2:  # [2]
                error = True
                break

            b = int(stack.pop())
            a = int(stack.pop())
            if exp[i] == '+':
                stack.append(a + b)
            elif exp[i] == '-':
                stack.append(a - b)
            elif exp[i] == '*':
                stack.append(a * b)
            elif exp[i] == '/':
                stack.append(a // b)  # [3]

    if error:
        print('#{} error'.format(tc))
    else:
        print('#{} {}'.format(tc, stack[-1]))
```
## Comments
[1] '.'을 만났을 때 stack에 숫자가 1개 넘게 있으면 error

[2] 연산자 계산을 해야하는데 stack에 숫자가 2개도 없으면 error

[3] a/b 로 해주면 실수값으로 계산이 진행되어서 fail이 뜬다.  
