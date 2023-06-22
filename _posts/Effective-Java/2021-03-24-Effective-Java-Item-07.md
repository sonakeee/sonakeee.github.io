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

title: "Effective Java Item 07"
excerpt: "다 쓴 객체 참조를 해제하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 가비지 컬렉터]
date: 2021-03-24
last_modified_at: 2021-04-06
---

# 다 쓴 객체 참조를 해제하라

## WeakHashMap을 사용해 캐시를 만들자

WeakHashMap은 key 값에 대해 약한 참조를 사용한다.  

강한 참조(Strong Reference): 일반적인 참조로 강한 참조를 가진 객체는 GC의 대상이 되지 않는다.  
부드러운 참조(Soft Reference): 부드러운 참조만 가진 객체는, 메모리가 부족하면 GC의 대상이 된다. 
메모리가 부족하지 않으면 GC의 대상이 되지 않는다.  
약한 참조(Weak Reference): 약한 참조만 가진 객체는 GC의 대상이 된다. 주로 캐시에 활용된다.

WeakHashMap을 사용해 캐시를 만들면 다 쓴 엔트리는 그 즉시 자동으로 제거될 것이다.

***
참고(Reference)  
[Java – Collection – Map – WeakHashMap (약한 참조 해시맵)](http://blog.breakingthat.com/2018/08/26/java-collection-map-weakhashmap/)

