---
title: 오버라이딩과 오버로딩의 차이점
category: daily
---

오버라이딩 (Overriding) 과 오버로딩 (Overloading) 이 다른거였다고?

맙소사.. 생각해보니 영어단어가 달랐다..

왜 같은 것이라고 생각했을까..



Overriding : '최우선시 되는' 이라는 형용사

상속에서 부모메서드를 자식에서 재정의하는 것을 오버라이딩이라고 한다. 

`부모클래스 이름 = new 자식클래스();` 

와 같이 선언하면 최우선시되는 자식클래스의 메서드가 실행되는 것을 볼 수 있다.



Overloading : '과적' (물건을 많이 싣는 것) 이라는 명사

같은이름의 메서드를 매개변수 (Parameter) 의 유형과 (Type) 개수를 달리하여 정의하는 것을 오버로딩이라고 한다.

자바는 똑똑해서 메서드이름이 같아도 매개변수의 유형과 개수가 다르면 구분하는 것을 볼 수 있다.