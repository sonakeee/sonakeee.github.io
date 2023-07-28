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

title: "docker (2) 명령어 image, container"
excerpt: "docker (2) 명령어 image, container"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Network
tags:
  - [Network, docker]
date: 2023-07-24
last_modified_at: 2023-07-24
---
docker 는 리눅스 컨테이너부터 시작된 기술 LXC(Linux Containers)

운영체제 수준 가상화

docker는 리눅스 커널에 LXC 기술을 사용해서 리눅스 컨테이너를 만들고 

그 컨테이너 상에 별도로 구성된 파일 시스템의 

시스템 설정 및 응용 프로그램을 실행할 수 있도록 하는 기술 


# docker engine 
- 서버/ 클라이언트 구조로 이루어짐 
- 서버는 데몬 프로세스 형태로 동작함 (데몬이란, 보통 계속 실행중인 프로그램)
- docker command 는 일종의 클라이언트 
- docker command 를 내리면 내부적으로 Rest API 를 사용해서 docker daemon process 를 호출하는 방식 
- docker ps 라고 명령하면 내부적으로 Rest API 로 호출함 

# docker image 
- docker 컨테이너를 생성하기 위한 명령을 가진 템플릿 
- 여러 이미지를 layer로 쌓아서 원하는 형태의 이미지를 만드는게 일반적

# docker container 
- docker image 가 리눅스 컨테이너 형태로 실행한 상태를 의미함 
- docker daemon 에 있는 커널에서 LXC 로 리눅스 컨테이너를 생성한 후 , 해당 컨테이너에 docker image 에 포함된 명령을 실행하고 docker container 를 만들고 실행함 
- docker container 는 분리된 공간, docker daemon process 를 통해 접속, 내부에서 코드수정, 재실행 가능


docker 는 image 와 container 를 다뤄서 작업함

# docker image 명령어
- docker image는  이미지명[:태그] 로 이루어지나 , 이미지명만 있는 경우엔 이미지명:latest 라고 생각하면된다.

- docker login -> 로그인 
- docker search ubuntu -> 이미지 검색 
- docker images -> 현재 다운된 docker image 를 확인할 수 있다.
- docker rmi [imageId] , docker image rm [imageID] -> docker image 를 삭제할 수 있다. 


# docker container 명령어 
- docker create ubuntu -> 컨테이너 생성 
- docker create --name [NAMES] ubuntu
- docker ps -> 실행중인 컨테이너만 확인 
- docker ps -a -> 모든 컨테이너 확인
- docker ps -a -q -> 컨테이너 아이디만 확인
- docker rm [NAMES]  - 컨테이너 삭제 
- docker start [NAMES] - 컨테이너 실행, 표준 입력을 받을 수 있는 상태라면 대기상태로 계속 실행 아니라면 종료됨
  (표준입력을 넣어줘야함 )



# run 과 start 의 차이

# run
- 이미지를 바탕으로 새로운 컨테이너를 띄우는 것
- docker run -it myubuntu

# docker run 의 옵션 
- -i 컨테이너 입력을 열어놓는 옵션
- -t 가상터미널을 할당하는 옵션   (-it = -i + -t)
- -name 컨테이너 이름을 설정하는 옵션
- docker run -it --rm --name myubuntu3 ubuntu  = exit 명령으로 종료 시 컨테이너도 자동 삭제됨
- 컨테이너를 백그라운드로 실행하기 docker run -it -d --name myubuntu ubuntu = 실행중인 상태지만 터미널로 연결은 안됨 , foreground 로 만들려면 docker attach myubuntu3 으로 들어갈 수 있음  



# start
- 중지된 컨테이너를 다시 시작 

# docker stop 
- docker container 의 status 를 exit , 실행중인 컨테이너의 실행을 잠깐 멈추는건 pause , 중지한 컨테이너는 start 로 재실행
- docker restart = 껐다가 키기 , 
- docker unpause 도 있음 




