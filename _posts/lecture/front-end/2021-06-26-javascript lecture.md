---
title: 자바스크립트 boost course
category: frontend
---

본 문서는 생활코딩 강의를 듣고 작성한 것입니다.

# 코드사전

**HTML**

| <input    | properties                    |
| --------- | ----------------------------- |
| type      | button, text                  |
| value     | 글                            |
| onclick   | 자바스크립트 코드             |
| onchange  | 값 변경 이벤트                |
| onkeydown | 버튼입력 (혹은 지우기) 이벤트 |

`<div>`

`<span>` 

**CSS**

| style       | properties |
| ----------- | ---------- |
| color       |            |
| class       |            |
| id          |            |
| font-weight |            |

**JavaScript**

| document                | properties                                              |
| ----------------------- | ------------------------------------------------------- |
| write                   | 글                                                      |
| querySelector('tag')    | ex) '#night_day': id 가 night_day 인 쿼리를 반환합니다. |
| querySelectorAll('tag') |                                                         |

| event     | properties  |
| --------- | ----------- |
| alert('') | 경고창 출력 |

**연산자**

> 공통점: 부등호, 줄바꿈
>
> 차이점: 비교연산자, 부등호(html)

**반복문**

> 공통점: if문, while문

**배열**

> 차이점: 대괄호, for-each

**함수**

> 차이점: 매개변수의 타입이 필요없다.

# 웹과 자바스크립트

우클릭 - 검사

Elements: 태그들이 있다.

JavaScript 는 사용자와 상호작용하는 언어

## 스크립트와 이벤트

script 태그에 사이에 자바스크립트 코드를 넣는다.

`<script>` 

`<input type="button" value="버튼"` 버튼태그

- `onclick="alert('hi')"` 클릭 시 이벤트

`<input type="text"` 입력창

- `onchange="alert('changed')"` 값 변경 이벤트
- `onkeydown="alert('key down')"` 버튼입력 (혹은 지우기) 이벤트

## 데이터타입

자바스크립트의 데이터타입에 대해 설명합니다.

**숫자**

> 콘솔에서 계산기를 사용할 수 있다.

**문자**

| str         | parameter | function                   |
| ----------- | --------- | -------------------------- |
| length      |           | 길이 반환                  |
| toUpperCase | ()        | 대문자 반환                |
| indexOf     | (String)  | 인덱스 반환<br />없으면 -1 |
| trim        | ()        | 공백 제거                  |

**변수**

`var` variable

---

**참고**

<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String>{:target="_blank"}


## CSS 기초와 선택자

css 의 속성: property

`<h1`

- `style="color:powderblue"` 

`<div` 어떠한 의미도 없는 태그: 자동 줄바꿈이 된다.

`<span` 의미없는 태그: 자동 줄바꿈이 없다.

- `style="font-weight:bold;"` 
- `class="js"` . 으로 호출: 여러대상을 그루핑합니다.
  `.js` 를 클래스선택자 (class selector) 라고합니다.
- `id="first"`  # 으로 호출: 한가지 대상을 식별합니다. id 를 우선시합니다.
  `#first` 를 id 선택자 (id selector) 라고 합니다.

**클래스명이 js 인  태그들의 스타일 변경**

```html
<style>
    .js{
	font-weight: bold;
	color: red;
    }
    #first{
        color: green;
    }
    span{
        color: blue;
    }
</style>
```

> 우선순위: id > class > span

`query` 질의하다.

```html
<input type="button" value="night" onclick="
    document.querySelector(selectors)
">
```

`document.querySelector(selectors);` 

**버튼클릭으로 배경바꾸기**

```html
<input type="button" value="검정배경" onclick="
    document.body.style.backgroundColor='black';
">
```

`document.querySelector('body').style.backgroundColor='black';` 같은 구문이다.

> 이벤트에서 `document` 를 사용해 자바스크립트를 작성할 수 있다.
>
> 스타일에서 css 선택자를 사용한다.

---

**참고**

<https://developer.mozilla.org/ko/docs/Web/API/Document/querySelector>{:target="_blank"}

# 제어문

date: 06.28

---

JavaScript 는 컴퓨터언어이며 프로그래밍 언어이다.

HTML 은 컴퓨터언어이지만 프로그래밍 언어가 아니다.

program: 순서를 만드는 것

programmer: 순서를 만드는 사람

## 조건문

conditional statements

**JavaScript**

`===` 비교연산자 (Comparison operator)

- java: `==` 

`<br>` 줄바꿈

- Java: `\n` 

```html
<script>
document.write(1===1);
</script>
```

**HTML**

`&lt;` < : 왼쪽으로 꺽쇠

`&gt;` > : 오른쪽으로 꺽쇠

> Java 와 동일한 것들: if문

> QuerySelector 에는 작은 따옴표 ( ' ' )
> 모든 곳에 큰 따옴표가 아닌 작은 따옴표를 사용할 것!!
> Java 는 문자에 작은따옴표 문자열에 큰따옴표를 사용하는 것과 다르다.

**토글연습 예제**

```html
<body>
  <h1>토글연습</h1>
  <input id="toggle" type="button" value="Black theme" onclick="
  if(document.querySelector('#toggle').value === 'Black theme'){
    body.style.backgroundColor = 'black';
    // document.querySelector('body') 와 body 는 같은 코드입니다.
    body.style.color = 'white';
    document.querySelector('#toggle').value = 'White theme';
  }else{
    body.style.backgroundColor = 'white';
    body.style.color = 'black';
    document.querySelector('#toggle').value = 'Black theme';
  }
  ">
  <br>
  <br>
  hello world!!
</body>
```

## 리팩토링

date: 06.29

Q. 리팩토링이란?

A. 사용자가 편리하게 사용할 수 있도록 기능을 개선하는 것.

`this` 자신의 태그를 가리킨다. 더 이상 id 를 쓸 필요가 없어진다.

`var` 바디태그와 같은 것들을 변수로 지정해도 된다.

**this 예제**

더 간결해진 코드를 다음과 같이 볼 수 있다.

```html
<body>
  <h1>토글연습</h1>
  <input type="button" value="Black theme" onclick="
  if(this.value === 'Black theme'){
    body.style.backgroundColor = 'black';
    body.style.color = 'white';
    this.value = 'White theme';
  }else{
    body.style.backgroundColor = 'white';
    body.style.color = 'black';
    this.value = 'Black theme';
  }
  ">
  <br>
  <br>
  hello world!!
</body>
```

# 함수

`function` 스크립트 태그 내에 넣는다.

Q. ul 이란?

A. \<ol> : ordered list 의 약자로 숫자나 알파벳 등 순서가 있는 목록을 만드는데 사용됩니다.

\<ul> : unordered list 의 약자로 순서가 필요없는 목록을 만듭니다.

\<dl> : definition list 의 약자로 사전처럼 용어를 설명하는 목록을 만듭니다.

Q. li 이란?

A. \<li> : list item 의 약자로 각 항목들을 나열할 때 사용합니다.

**예시**

```html
<ul>
    <li>영어</li>
    <li>수학</li>
    <li>과학</li>
</ul>
```

parameter (매개변수) : function sum(left, right)

argument (인자) : sum(3, 4)

# 객체

배열은 대괄호, 객체는 중괄호로 선언

```javascript
var coworkers = {
    "programmer": "egoing",
    "designer": "leezche"
}
```

**for-each 구문** 

```javascript
for(var key in coworkers) {
    document.write(key + ': ' + coworkers[key] + '<br>');
}
```

**메서드 정의** 

```javascript
coworkers.showAll = function(){
    for(var key in this) {
    	document.write(key + ': ' + this[key] + '<br>');
	}
}

var showAll = function(){
}
```

`this` 를 위와 같이 사용가능합니다.

property: 객체에 소속된 변수를 말합니다.

```javascript
var Body = {
    setColor: function (color){
        body.style.color = color;
    	},
    setBackgroundColor: function (color){
        body.style.backgroundColor = color;
	    }
    }
}
```

# 활용

## 자바스크립트 파일분리

date: 06.30

이때까지는 html 파일 내에 \<script> 태그를 추가하여 JavaScript 를 구현하였다.

하지만 \<script> 태그는 그 html 파일 내에서만 작동하므로 모든 html 에 \<script> 태그를 붙여줘야만 했다.

자바스크립트 파일을 분리하여 저장함으로써 코드반복을 줄일 수 있다.

> body 객체는 html 에서는 사용 가능하지만 JavaScript 에서는 사용할 수 없으므로 document.querySelector('body') 를 사용한다.

```html
<input type="button" value="Black theme" onclick="
blackTheme();
  ">
```
**객체와 함수구현** 

```javascript
var Body = {
  setColor: function (color) {
    document.querySelector('body').style.color = color;
  },
  setBackgroundColor: function (color) {
    document.querySelector('body').style.backgroundColor = color;
  }
}

function blackTheme() {
  if (this.value === 'Black theme') {
    Body.setBackgroundColor('black');
    Body.setColor('white');
    this.value = 'White theme';
  } else {
    Body.setBackgroundColor('white');
    Body.setColor('black');
    this.value = 'Black theme';
  }
}
```

## jQuery

오랜기간 모두가 써온 라이브러리

Q. 라이브러리란? (library) 

필요한 것을 가져올 수 있는 도서관

Q. 프레임워크란? (framework)

공통적인 것을 만드는 틀

**jQuery 다운로드방법**

<https://jquery.com/>{:target="_blank"}

1. Download 탭 클릭
2. Download the compressed, production jQuery 버전 을 클릭

혹은 CDN 이용방법

1. 아래에 Google CDN 링크클릭
2. jQuery 에 3.x snippet 이 최신버전이므로 (21.06.30 기준) \<script 태그를 복사
3. html 에 추가

Q. CDN 이란?

콘텐츠 전송 네트워크 (Content Delivery Network) 의 약자

원본을 jQuery 서버에 보관하고 우리는 \<script src 를 통해서 불러오는 것

---

**반복문을 이용해 링크 색 바꾸기**


```javascript
var Links = {
  setColor: function (color) {
    var alist = document.querySelectorAll('a');
    var i = 0;
    while (i < alist.length){
      alist[i].style.color = color;
      i = i + 1;
    }
  }
}
```

jQuery 는 $ 를 사용한다.

**jQuery 를 사용해 링크 색 바꾸기** 

```javascript
var Links = {
  setColor: function (color) {
    $('a').css('color', color);
  }
}
```

위와 같은 코드입니다.

Q. a 태그란?

A. anchor 를 뜻한다. 링크로서의 기능과 해당 id 로 이동하는 기능이 있다.

## 추천할만한 객체

date: 07.01

**UI vs API**

UI (User Interface)

API (Application Programming Interface)

**객체**

1. document 객체를 찾아볼 것: document 의 객체는 DOM (Document Object Model)
2. window 객체를 찾아볼 것 
3. ajax 객체
4. cookie 현재 상태 유지하는 것: 개인화하는 것
5. offline web application
6. webRTC: 화상통신
7. speech: 사용자 음성인식
8. webGL: 3차원 그래픽
9. webVR: 가상현실
