---
title: "[JS] Variables"
layout: post
category: blog
headerImage: false
tags:
  - var
  - let
  - const
author: Junyeol Lee
---

### var, let, const 비교 정리 

- 선언한 변수 다시 선언하기

  - var : 허용한다.

    ```javascript
    var foo = "bar";
    var foo = "bar";
    ```

  - let, const : 허용하지 않는다.

    ```javascript
    let foo = "bar";
    let foo = "bar";
    VM195:1 Uncaught SyntaxError: Identifier 'foo' has already been declared
        at <anonymous>:1:1
    ```

- 호이스팅: 변수의 선언부와 할당문을 구분하여, 변수의 선언문을 모두 끌어올려 초기화 하는 것

  - var : (호이스팅 O) 변수의 선언과 동시에 초기화된다.

    ```javascript
    console.log(foo); // undefined
    var foo;
    console.log(foo); // undefined
    foo = 5;
    console.log(foo); // 5
    ```

  - let, const : (호이스팅 X) 변수의 선언은 되었지만, 초기화는 되지 않는다.

    ```javascript
    //console.log(foo); -> Uncaught ReferenceError: foo is not defined
    let foo;
    console.log(foo); // undefined
    foo = 5;
    console.log(foo); // 5
    ```

  ※ 이에 관한 자세한 설명: https://jaewonism.com/posts/11

- Scope

  - var : 함수 스코프
  - let : 블록 스코프




※ 참조 : http://blog.nekoromancer.kr/2016/01/26/es6-var-let-%EA%B7%B8%EB%A6%AC%EA%B3%A0-const/