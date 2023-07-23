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

title: "nginx 적용기 (2) nginx 와 apache"
excerpt: "nginx 적용기 (2) nginx 와 apache"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
- Nginx
  tags:
- [Nginx, apache]
  date: 2023-07-20
  last_modified_at: 2023-07-20
---

# apache
- apache 는 접속 요청이 들어올 때(네트워크 소켓이 필요할 때마다) 프로세스 또는 스레드를 새로 만들어서 배당하는 구조를 가지고 있다.
- 하지만 이런 방법은 C10K 문제를 야기하였고 (동시에 열리는 소켓수가 10000개 이상이면 웹 서버의 성능이 급격히 저하되는 현상) 그로인해 Nginx 가 등장하게 되었다.
- 프로세스가 너무 많이 생성되어 메모리 부족현상이 나타났다.

# preFork 방식
- apache 의 새로운 프로세스를 만드는 방식은 오래걸려서 요청이 들어오기전에 프로세스를 미리 만드는 preFork 방식을 사용하였다.
- 모듈을 만들고 서버에 추가하기 쉽다는 장점이 있고 , 아파치 서버가 동적 컨텐츠를 쉽게 처리할 수 있게 해준다.



이를 극복하기 위해서 비동기 이벤트 기반의 싱글 스레드 방식으로

(소켓이 필요할 때 프로세스나 스레드를 새로 만들지 않고 하나의 스레드가 모든 요청을 처리하되 이벤트가 발생할때만 해당 소켓의 요청을 처리함 )


# Nginx 의 등장 
- 처음 nginx 는 apache 와 함께 사용하였다고 한다. 

# Master Process 
- nginx.conf 라는 설정파일에 작성한 내용을 읽고 설정에 맞게 worker Process 를 생성하는 프로세스 


# Worker Process 
- 실제로 일을 하는 프로세스 , 
- 생성될 때 listen Socket 을 지정받음 그 소켓에 클라이언트의 요청이 들어오면 커넥션을 생성하고 요청을 처리함  
- 연결된 커넥션은 keep-alice 시간만큼 끊기지 않고 유지됨,
- 커넥션이 생성되었다고 해서 worker process 가 해당 커넥션 하나만을 담당하지는 않음 
- 생성된 커넥션에 아무런 요청이 없는 상태일 때 , 새로운 커넥션이 들어오면 또 연결이 가능함,
- 커넥션의 생성 , 제거 , 요청을 처리하는 행위를 모두 Event 라고 부름 

# 비동기적 Event 처리방식 
- event 들은 os 커널이 queue 형태로 worker process 에 전달한다.



아파치 서버는 요청이 없으면 방치되는 프로세스가 있지만 nginx 는 이벤트 queue 를 통해 효율적으로 자원을 사용한다 

# thread pool 
- 위의 처리방식은 차례대로 순서를 기다려야한다는 문제가 있다. 
- worker process 가 요청을 이벤트 큐에 넣으면 각 요청들을 thread pool 에 있는 thread 들이 이벤트를 처리해서 결과를 worker process 에 전달해줌 
- 1.7.11 버전 이상에선 별도의 설정이 필요함 


# Context Switching
- nginx 는 cpu 하나당 worker process 하나만을 담당해서 Event 기반 구조라고 한다.  
- 하나의 cpu 가 여러개의 프로세를 담당하는 apache 와의 가장 큰 차이점이다  
- 이렇게 cpu 하나의 cpu 가 프로세스를 옮겨다니는것을 Context Switching 이라고하는데 그것을 줄일 수 있다. 

# nginx 의 설정 변경 
- nginx 의 설정을 서버를 중단하지 않고도 바꾸는 것을 가능하게 함
- nginx 의 설정을 변경하면 master process 는 그 설정에 맞는 worker process 를 따로 생성함 


# SSL 터미네이션 기능 
- Nginx가 클라이언트와는 https 통신을 하고 서버와는 http 통신을 하는 것 



