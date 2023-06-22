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

title: "Effective Java Item 45"
excerpt: "스트림은 주의해서 사용하라"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Effective Java
tags:
  - [Effective Java, Stream, lambda]
date: 2021-05-05
last_modified_at: 2021-05-05
---
# 스트림은 주의해서 사용하라

## 람다로 불가능한 것들

p 273 를 보면

> 코드 블록에서는 return 문을 사용해 메서드에서 빠져나가거나, 
> break나 continue 문으로 블록 바깥의 반복문을 종료하거나 반복을 한 번 건너뛸 수 있다. 
> 또한 메서드 선언에 명시된 검사 예외를 던질 수 있다. 하지만 람다로는 이 중 어떤 것도 할 수 없다.

라고 나와있다.

`메서드 선언에 명시된 검사 예외를 던진다` 라는 문장의 원문이다.
> throw any checked exception that this method is declared to throw;

그런데 [링크](https://www.slipp.net/questions/572) 를 참조하면 람다로도 checked exception을 
구현할 수 있는 거 같다(?)

return으로 빠져나오는 것도 가능하지 않을까?

```java
(int a, int b) -> {
  if(a==1){
    return 0 ;
  }
return a+b;
}
```

비슷하게
```java
(int a, int b) -> {
  while(true){
    break;
  }
}
```
이런 것도 가능할 거 같다.

책에서 말한 람다로 할 수 있는 것이란 이렇게 중괄호 속에 복잡하게 표현한 것은 제외한 것일까?
좀더 공부해보아야겠다.

***
참고(Reference)  
[자바 8 람다에서 checked exception을 어떻게 구현하면 좋을까?](https://www.slipp.net/questions/572)  
[Handling checked exceptions in Java streams](https://www.oreilly.com/content/handling-checked-exceptions-in-java-streams/)