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

title: "모던 자바스크립트 (19) 화살표 함수 다시, 프로퍼티 플래그 , Getter, setter"
excerpt: "모던 자바스크립트 (19)  화살표 함수 다시, 프로퍼티 플래그 , Getter, setter"
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
# 화살표 함수 
- 단순히 짧게 쓰기 위해 사용하는것이 아님 
- this 가 없음 , new 와 함께 실행할 수 없음 , 화살표함수는 어떤것도 바인딩 시키지않음,
- 화살표함수에서 this 를 사용하면 일반 변수 서칭과 마찬가지로 this 의 값을 외부 렉시컬 환경에서 찾는다 
- arguments 가 없음 , 모든 인수에 접근할 수 있게 해주는 유사배열객체 arguments 를 지원하지 않음 
- 위와 같은 특징은 this 값과 arguments 정보를 함께 실어 호출을 포워딩해 주는 데코레이터를 만들 때 유용하게 사용됨 
```js
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('안녕, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수"가 출력됩니다.


function defer(f, ms) {
    return function(...args) {
        let ctx = this;
        setTimeout(function() {
            return f.apply(ctx, args);
        }, ms);
    };
}
```
- 위의 함수 두개를 보고 어떠한 차이가 있는지 보기

# 프로퍼티 플래그 
- 값과 함께 플래그라 불리는 특별한 속성 세가지를 가짐 
- writable - true 이면 값을 수정할 수 있음, 그렇지않으면 읽기만 가능 
- enumerable - true 이면 반복문을 사용해 나열 가능, 그렇지 않다면 반복문을 사용해 나열 가능 
- configurable - true 이면 프로퍼티 삭제나 플래그 수정이 가능함 , 그렇지 않으면 프로퍼티 삭제와 플래그 수정이 불가능함 
- 특별한 경우가 아니고선 모든 Flag 가 true 가 됨 , 이 flag 는 언제든 수정할 수 있음


# Object.getOwnPropertyDescriptor(obj, propertyName)
```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);

let user = {
    name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```
- obj - 얻고자 하는 객체 
- propertyName - 정보를 얻고자 하는 객체 내 프로퍼티 
- 메서드를 호출하면 프로퍼티 설명자라고 불리는 객체가 반환되는데 여기에는 프로퍼티 값과 세 플래그에 대한 정보가 모두 담김

# Object.defineProperty(obj, propertyName, descriptor)
- obj, propertyName - 설명자를 적용하고 싶은 객체와 객체 프로퍼티 
- descriptor - 적용하고자 하는 프로퍼티 설명자 
- 이 메서드는 객체에 해당 프로퍼티가 있으면 플래그를 원하는 대로 변경해줌, 프로퍼티가 없으면 인수로 넘겨받은 정보를 이용해 새로운 프로퍼티를 만듬, 플래그 정보가 없으면 플래그 값은 false

```js
let user = {};

Object.defineProperty(user, "name", {
  value: "John"
});

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */
```


# writable 플래그 
- user.name에 값을 쓰지 못하도록 만들기 
```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete"; // Error: Cannot assign to read only property 'name'
```
- 에러는 엄격모드에서만 나타남 

- enumerable 플래그 
- configurable 플래그 등등 있음 

# 프로퍼티 getter 와 setter
- 프로퍼티는 두 종류로 나뉨, 이전까지 배운것은 데이터 프로퍼티 , getter 와 setter 는 접근자 프로퍼티로이다
- 접근자 프로퍼티의 본질은 함수이다. 값을 획득하고 설정하는 역할을 담당 

```js
let obj = {
  get propName() {
    // getter, obj.propName을 실행할 때 실행되는 코드
  },

  set propName(value) {
    // setter, obj.propName = value를 실행할 때 실행되는 코드
  }
};
```
- getter 는 읽으려고 할때 실행, setter 는 값을 할당할 때 사용 
- getter 와 setter 메서드를 구현하면 객체엔 fullName 이라는 가상의 프로퍼티가 생김, 읽고 쓸순 있지만 실제로 존재하지는 않음 
```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

// 주어진 값을 사용해 set fullName이 실행됩니다.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

# 접근자 프로퍼티 설명자 
- 접근자 프로퍼티엔 설명자 value 와 writable 이 없고 get 과 set 이라는 함수가 있음 
- get - 인수가 없는 함수  , 프로퍼티 읽을 때 동장 
- set - 인수가 하나인 함수 - 프로퍼티에 값을 쓸 때 호출 
- enumerable , configurable - 데이터 프로퍼티와 동일 

# getter 와 setter 사용하기 
```js
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("입력하신 값이 너무 짧습니다. 네 글자 이상으로 구성된 이름을 입력하세요.");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // 너무 짧은 이름을 할당하려 함
```
