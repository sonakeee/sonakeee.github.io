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

title: "Effective Java Item 02"
excerpt: "생성자에 매개변수가 많다면 빌더를 고려하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 빌더 패턴]
date: 2021-03-19
last_modified_at: 2021-03-31
---

# 생성자에 매개변수가 많다면 빌더를 고려하라

## 일관성이 무너진 상태란?

객체가 완전히 생성되기 전 초기화가 덜 된 상태를 말한다.  
사용자가 실수로라도 완전히 생성되기 전의 객체를 사용해버리면 런타임 에러가 발생할 수 있다.

## 물리적으로 멀리 떨어진 코드란?

여기서 물리적으로 떨어진 두 대상은 "버그의 원인이 되는 코드"와 "버그가 발생한 코드"의 위치이다.  
런타임 에러 코드가 발생되는 곳은 초기화를 하는 코드부분이 아니라, 불완전한 객체를 넘겨받아서 그 객체를 구동하는
코드이다. 이 둘의 코드는 전혀 다른 클래스, 혹은 심지어 전혀 다른 패키지 일 수도 있으므로 코드가 물리적으로 멀리
떨어져 있다고 하는 것이다.

## 명명된 선택적 매개변수(named optional parameters)

[참조링크](http://blog.naver.com/PostView.nhn?blogId=dktmrorl&logNo=221908298329&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)