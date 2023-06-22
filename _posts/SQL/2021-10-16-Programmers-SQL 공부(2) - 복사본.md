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

title: "[Programmers] SQL 공부(2)"
excerpt: "[Programmers] SQL 공부(2)"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - SQL 
tags:
  - [SQL, Programmers]
date: 2021-10-16
last_modified_at: 2021-10-16
---
Problem URL : [역순 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59035)

```sql
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC
```

Problem URL : [최솟값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59038)

```sql
SELECT MIN(DATETIME) FROM ANIMAL_INS
```

Problem URL : [동명 동물 수 찾기](https://programmers.co.kr/learn/courses/30/lessons/59041)

```sql
SELECT NAME, COUNT(NAME) "COUNT"
FROM ANIMAL_INS 
GROUP BY NAME 
HAVING COUNT(NAME) >= 2 
ORDER BY NAME
```

Problem URL : [이름이 있는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59407)

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID
```

Problem URL : [있었는데요 없었습니다](https://programmers.co.kr/learn/courses/30/lessons/59043)

```sql
SELECT A.ANIMAL_ID, A.NAME 
FROM ANIMAL_INS A 
LEFT JOIN ANIMAL_OUTS B 
ON A.ANIMAL_ID = B.ANIMAL_ID 
WHERE A.DATETIME > B.DATETIME
ORDER BY A.DATETIME
```

Problem URL : [이름에 el이 들어가는 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59047)

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE NAME LIKE '%EL%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```

