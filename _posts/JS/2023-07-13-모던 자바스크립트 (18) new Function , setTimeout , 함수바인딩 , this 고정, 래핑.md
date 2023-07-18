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

title: "모던 자바스크립트 (18) new Function , setTimeout , 함수바인딩 , this 고정, 래핑"
excerpt: "모던 자바스크립트 (18) new Function , setTimeout , 함수바인딩 , this 고정, 래핑"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - JS
tags:
  - [JS, 모던자바스크립트]
date: 2023-07-18
last_modified_at: 2023-07-18
---
# new Function 문법
- 자주 사용하는 문법은 아님, 대안이 없을경우에만 사용 
```js
let func = new Functin([arg1, arg2, ...argN], 'alert("hello")' );
```
- 기존의 함수 사용방법과 new Function 의 차이는 런타임에 받은 문자열을 사용해서 함수를 만들 수 있다. 
- new Function 은 어떤 문자열도 함수로 바꿀 수 있음
- 서버에서 코드를 받거나 템플릿을 사용해 함수를 동적으로 컴파일해야하는경우 등 아주특별할때 사용 

- 함수는 특별한 프로퍼티 [[Environment]]에 저장된 정보를 이용해 자기 자신이 태어난 곳을 기억하고 함수가 만들어진 렉시컬 환경을 참조함
- 그런데 new Function 을 이용해 함수를 만들면 [[Environment]] 프로퍼티가 현재 렉시컬 환경이 아닌 전역 렉시컬 환경을 참조하게 됨 
- new Function 을 이용해 만든 함수는 외부 변수에 접근할 수 없고 , 오직 전역 변수에만 접근할 수 있음

- minify 가 문제를 일으킬 수 있음, 압축기가 동작한 이후엔 new function 으로 만든 함수 내부에서 외부 렉시컬 환경에 접근하려고 할 때 문제가 발생할 수 있음
- 위와 같은 이유로 new Function 에서 무언가를 넘겨주려고 할 땐 인수를 통해 사용해야한다 

# setTimeout
- 일정 시간이 지난 후에 함수를 실행 
```js
function sayHi() {
}
setTimeout(sayHi, 1000, "홍길동", "안녕하세요.");
setTimeout(() => alert('안녕하세요.'), 1000);
```
- clearTimeout 으로 스케줄링 취소할 수 있음 
- setTimeout 의 대기 시간을 0 으로 하면 함수를 가능한 한 빨리 실행할 수 있음, 실행이 종료된 직후에 원하는 함수가 실행될 수 있게 함
- setTimeout 의 함수를 실행하는 시간도 포함되기때문에 1000보다 빠를수도있고 스택문제로 1000보다 느릴수도 있다 

# setInterval
- 일정 시간 간격을 두고 함수를 실행하는 방법 
- setTimeout 과 동일한 문법과 동일한 인수를 사용하지만 함수를 주기적으로 사용한다는 점이 다름 
```js
// 2초 간격으로 메시지를 보여줌
let timerId = setInterval(() => alert('째깍'), 2000);

// 5초 후에 정지
setTimeout(() => { clearInterval(timerId); alert('정지'); }, 5000);
```

html5 표준에서 다섯번재 중첩 타이머 이후엔 대기 시간을 최소 4밀리초 이상으로 강제해야 한다는 제약이 있다 

# 코드 변경 없이 캐싱 기능 추가하기 
```js
function slow(x) {
  // CPU 집약적인 작업이 여기에 올 수 있습니다.
  alert(`slow(${x})을/를 호출함`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function(x) {
    if (cache.has(x)) {    // cache에 해당 키가 있으면
      return cache.get(x); // 대응하는 값을 cache에서 읽어옵니다.
    }

    let result = func(x);  // 그렇지 않은 경우엔 func를 호출하고,

    cache.set(x, result);  // 그 결과를 캐싱(저장)합니다.
    return result;
  };
}

slow = cachingDecorator(slow);

alert( slow(1) ); // slow(1)이 저장되었습니다.
alert( "다시 호출: " + slow(1) ); // 동일한 결과

alert( slow(2) ); // slow(2)가 저장되었습니다.
alert( "다시 호출: " + slow(2) ); // 윗줄과 동일한 결과
```
- 모든 함수를 대상으로 데코레이터를 호출 할 수 있는데 이 때 반환되는것은 캐싱 래퍼임, 캐싱이 가능한 함수를 원하는 만큼 구현할 수 있어서 유용하게 사용가능함 

# func.call(context, ...args)
- this 를 명시적으로 로정해 함수를 호출할 수 있게 해주는 특별한 내장 함수 메서드 



데코레이터는 함수를 감싸는 래퍼로 함수의 행동을 변화시킴, 주요 작업은 여전히 함수에서 처리 

# 함수 바인딩
- 객체 매서드를 콜백으로 전달할 때 this 정보가 사라지는 문제가 생김 
```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(user.sayHi, 1000); // Hello, undefined!
```
- 이렇게 되는 이유는 setTimeout 에 객체에서 분리된 user.setTimeout 이 전달되어서 this 가 undefined 가 나옴, 
- setTimeout 에서 this 를 window 로 호출하게 됨 

# 래퍼  
```js
let user = {
    firstName: "John",
    sayHi() {
        alert(`Hello, ${this.firstName}!`);
    }
};

setTimeout(function() {
    user.sayHi(); // Hello, John!
}, 1000);
```
- 래퍼 함수를 통해 문제를 해결하는 방법 , 외부 렉시컬 환경에서 user를 받아서 메서드를 호출함 


# 바인드 func.bind(context)
- 모든 함수는 this 를 수정하게 해주는 내장 메서드 bind 를 제공함. 
```js
let boundFunc = func.bind(context);

let user = {
    firstName: "John",
    say(phrase) {
        alert(`${phrase}, ${this.firstName}!`);
    }
};

let say = user.say.bind(user);

say("Hello"); // Hello, John (인수 "Hello"가 say로 전달되었습니다.)
say("Bye"); // Bye, John ("Bye"가 say로 전달되었습니다.)
```
- 함수처럼 호출 가능한 특수 객체를 반환함 , 이 객체를 호출하면 this 가 context 로 고정된 함수 func 가 바노한됨 

# 부분적용 
- this 뿐만 아니라 인수도 바인딩이 가능
```js
let bound = func.bind(context, [arg1], [arg2], ...);

function mul(a, b) {
    return a * b;
}

let triple = mul.bind(null, 3);

alert( triple(3) ); // = mul(3, 3) = 9
alert( triple(4) ); // = mul(3, 4) = 12
alert( triple(5) ); // = mul(3, 5) = 15
```


this 를 고정하기 위해 사용하는 방법에 대해서 좀 생각해보는게 좋을것같다. 