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

title: "[Programmers] SQL 공부(3)"
excerpt: "[Programmers] SQL 공부(3)"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - SQL 
tags:
  - [SQL, Programmers]
date: 2021-10-22
last_modified_at: 2021-10-22
---
Problem URL : [아픈 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59036)

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = "SICK"
```

Problem URL : [동물 수 구하기](https://programmers.co.kr/learn/courses/30/lessons/59406)

```sql
SELECT COUNT(*)
FROM ANIMAL_INS;
```

Problem URL : [입양 시각 구하기(1)](https://programmers.co.kr/learn/courses/30/lessons/59412)

### HAVING 절 이용
```sql
SELECT HOUR(DATETIME) HOUR, COUNT(DATETIME) COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR
HAVING HOUR >= 9 and HOUR <= 19
ORDER BY HOUR(DATETIME)
```

### WHERE 절 이용
```sql
SELECT HOUR(DATETIME) HOUR, COUNT(DATETIME) COUNT
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) >= 9 AND HOUR(DATETIME) <= 19
GROUP BY HOUR
ORDER BY HOUR(DATETIME)
```

Problem URL : [입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413)

```sql
SET @hour := -1;

SELECT (@hour := @hour + 1) as HOUR,
    (SELECT COUNT(*) 
     FROM ANIMAL_OUTS 
     WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23
```
단순히 GROUP BY HOUR(DATETIME)을 사용해 COUNT(HOUR(DATETIME))을 출력하면
1건 이상 존재하는 시간만 출력된다 (0~23 중 0건인 시간도 출력되어야 한다.)


Problem URL : [NULL 처리하기](https://programmers.co.kr/learn/courses/30/lessons/59410)

```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

