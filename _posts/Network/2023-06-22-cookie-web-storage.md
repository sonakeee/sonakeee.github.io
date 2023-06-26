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

title: "쿠키, 로컬스토리지, 세션스토리지에 대해서"
excerpt: "쿠키, 로컬스토리지, 세션스토리지에 대해서"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Network
tags:
  - [Network]
date: 2023-06-26
last_modified_at: 2023-06-26
---
# Local Storage, session Storage, Cookie 의 차이점

난 로그인 기능 등 많은 요소들을 로컬스토리지보단 쿠키에 의존해왔었다  

하지만 로컬스토리지 , 세션스토리지 등을 사용하라는 요청속에서

왜? 라는 의문과 함께 어떤 차이점이 있는지 알아보려 한다  

&nbsp;

받은 요청  
token 값은 쿠키보단 세션 스토리지에 저장,    
api 호출시마다 불러와서 header 값에 세팅     
Bearer 는 저장할때 포함해서 저장하기보다는 호출할떄 세팅하기      

&nbsp;  
&nbsp;  

# 사용 이유
HTTP는 요청과 응답으로 이루어지는 사이클이 끝나면 연결이 끊어짐 = 무상태성을 띈다고 함,   
결과적으로 클라이언트의 상태를 보존하지 않게 됨   
클라이언트의 상태를 서버가 아닌 클라이언트에 저장하고 필요시마다 데이터를 꺼내쓰기 위해 사용   

&nbsp;  

&nbsp;

# Cookie
만료기한이 있는 키-값 저장소   
동일한 서버에 다시 요청을 하면 쿠키 안에있는 도메인 속성값을 참조함   
데이터 유효기간 지정 가능    
매우 작은 데이터 저장 용량(4kb)
문자열만 저장 가능 &nbsp;   
세션관리 - 서버가 알아야 할 정보 , 로그인 및 사용자정보, 접속시간   
개인화 - 사용자에 맞는 페이지 제공   
트래킹 - 사용자 행동 및 패턴 분석   
이러한 목적을 가지고 있다   
XSS 공격과 CSRF 공격 등에 취약하기 때문에 HttpOnly 옵션을 켜두고,   
쿠키를 사용하는 요청은 서버 단에서 검증하는 로직을 꼭 마련해둬야함   


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Web Storage
로컬 스토리지와 세션 스토리지를 합쳐서 web storage 라고한다 &nbsp;
&nbsp; 
HTML5에서 추가된 저장소 2MB 이상의 정보를 저장 가능하여 쿠키보다 많은 용량을 저장할 수 있다. &nbsp;
&nbsp; 
문자열 외에도 자바스크립트의 모든 원시형 데이터와 객체 저장 가능 &nbsp;
&nbsp;
유효 범위와 보존 기간에 따라 local 과 session 으로 나뉜다 &nbsp;
&nbsp;
window 객체의 프로퍼티로 존재 &nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# Local Storage
window.localStorage 로 사용 &nbsp;
&nbsp;
사용자가 지우지 않는이상 브라우저에 남아있다 &nbsp;
&nbsp;
웹 도메인당 1개씩 생성되며 새 창을 띄워도 도메인만 같으면 동일한 데이터를 공유하게 됨 &nbsp;
다른 도메인의 로컬스토리지에는 접근이 불가능 &nbsp;
&nbsp;
쿠키를 이용하지 않는 앱 환경에서 자동로그인등에 주로 사용 &nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# session Storage
window.sessionStorage 로 사용 &nbsp;
사용자가 브라우저 탭을 닫을경우 제거된다 &nbsp;
&nbsp;
새 창, 새 탭의 단위로 스토리지가 생성되며 데이터를 공유하지 않는다. &nbsp;
같은 탭이라도 도메인이 다르면 또 다른 세션스토리지가 생성됨 &nbsp;
&nbsp;
도메인, 윈도우 탭 별 독립적인 공간,  회원가입 입력 폼 , 일회성 로그인에 사용됨 &nbsp;

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 여담
그 외에도 session (web storage 의 session 이 아님)이 있는데 &nbsp;
유저가 인증을 할 때 서버는 이 기록을 서버에 저장해야함 이를 세션이라고 함 &nbsp;
서버의 리소스를 사용함, 세션에 대해선 이정도로만 알고 다음 기회에 알아보는걸로 ... &nbsp;
&nbsp;

Bearer 는 저장할때 포함해서 저장하기보다는 호출할떄 세팅하기 &nbsp;
이에 대한 이유는 bearer을 쿠키나 스토리지에 함께 사용하면 그 값이 JWT 토큰 즉 API 인증에 이용하는 값이란걸 대놓고 공표하는 것이기 때문에 &nbsp; 
그래서 값을 저장하더라고 그 값이 어떤 용도인지를 모르게 하는게 보안상 중요합니다. &nbsp;
이러한 이유 때문이었다. 
