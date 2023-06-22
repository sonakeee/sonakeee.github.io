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

title: "Effective Java Item 06"
excerpt: "불필요한 객체 생성을 피하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, 불변, 어댑터]
date: 2021-03-23
last_modified_at: 2021-04-04
---

# 불필요한 객체 생성을 피하라

## 어댑터 패턴

p33 이 헷갈려서 원문을 찾아봤다

> Because an adapter has no state beyond that of its backing object, there’s no need to create
more than one instance of a given adapter to a given object.

어댑터가 하나보다 더 생성될 필요가 없다는 뜻이다.  
어댑터에 위임된 객체말고는 다른 상태를 가지지 않아서 그렇다.  
실제로 Map의 keySet 메서드로 반환되는 Set 어댑터(뷰)는 매번 동일하다.  
모두 Map 인스턴스를 대변하는 어댑터이기 때문이다.

