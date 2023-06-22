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

title: "Effective Java Item 43"
excerpt: "람다보다는 메서드 참조를 사용하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, finalizer, cleaner]
date: 2021-05-05
last_modified_at: 2021-05-05
---
# 람다보다는 메서드 참조를 사용하라

## 비한정적 참조

String::compareToIgnoreCase  

(s1, s2) -> s1.compareToIgnoreCase(s2)

에서 수신 객체 전달용 매개변수는 s1, 그 뒤에는 메서드 선언에 정의된 매개변수

함수 객체를 적용하는 시점에 수신 객체를 알려준다.(s1)

`한정적 참조는 아니라는 뜻 같은데, 미리 알려준단건지 알려주지 않는다는 건지 모르겠다...` 


***
참고(Reference)  
[Method References](https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)  
[https://bit.ly/2uYQnbh](https://bit.ly/2uYQnbh)