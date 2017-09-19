---
title: "[Spring] ArgumentResolver"
layout: post
category: blog
headerImage: false
author: Junyeol Lee
tags:
  - ArgumentResolver
  - Login
  - parameter
  - argument
---

HandlerMethodArgumentResolver는 컨트롤러로 들어온 요청에 대한 매개변수(Parameter)를 
처리해 주는 인터페이스다.

```java
package org.springframework.web.method.support;

import org.springframework.core.MethodParameter;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;

/**
 * 컨트롤러로 들어온 요청에 대해 메소드 매개변수를 Argument 값으로 바꿔주는 인터페이스이다.
 */
public interface HandlerMethodArgumentResolver {

	/**
	 * 주어진 매개변수가 해당 리졸버가 지원하는지 체크한다.
	 * @param parameter 체크할 메소드 파라미터
	 * @return {@code true} 체크를 통과하면 true
	 * {@code false} 아니라면 false
	 */
	boolean supportsParameter(MethodParameter parameter);

	/**
	 * 받은 요청으로부터 매개변수를 Argument로 바꿔준다.
	 * A ModelAndViewContainer는 해당 요청에 대한 모델에 접근할 수 있게 한다.
     * A WebDataBinderFactory는 데이터 바인딩과 타입 변환 목적이 필요할 때, WebDataBinder 인스턴스를 생성할 수 있게 한다.
	 * @param parameter the method parameter to resolve. This parameter must
	 * have previously been passed to {@link #supportsParameter} which must
	 * have returned {@code true}.
	 * @param mavContainer 현재 요청에 대한 ModelAndViewContainer
	 * @param webRequest 현재 Request
	 * @param binderFactory WebDataBinder 인스턴스를 만들기 위한 팩토리
	 * @return 리턴 값은 Argument value 또는 null
	 * @throws Exception in case of errors with the preparation of argument values
	 */
	Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
			NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception;
}
```

HandlerMethodArgumentResolver가 무슨 역할을 하는 지 알겠다. 

이걸 우리 프로젝트에서는 세션에서 유저 객체를 받는 코드를 일일이 구현하지 않고, 
Resolver에서 처리해 넘겨주도록 구현하였다.

```java
public class AuthUserArgumentResolver implements HandlerMethodArgumentResolver {
	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		AuthUser loginUser = parameter.getParameterAnnotation(AuthUser.class);
		if (loginUser == null) {
			return false;
		} else {
			return true;
		}
	}

	@Override
	public Object resolveArgument(MethodParameter parameter,
			ModelAndViewContainer mvcContainer, NativeWebRequest webRequest,
			WebDataBinderFactory binderFactory) throws Exception {
		HttpServletRequest request = (HttpServletRequest) webRequest
				.getNativeRequest();
		HttpSession session = request.getSession();
		User user = (User) session.getAttribute("user");
		return user;
	}
}
```

살펴보자.

supportsParameter에서 파라미터를 체크한다. 파라미터가 AuthUser라는 Annotation을 갖고 있다면 true를 반환하고,
resolveArgument를 수행한다.

resolveArgument는 supportsParameter가 true를 반환한 Parameter에 대해서 Argument로 처리한다.


```java
	@Override
	public void addArgumentResolvers(
			List<HandlerMethodArgumentResolver> argumentResolvers) {
		argumentResolvers.add(new AuthUserArgumentResolver());
	}
```

이렇게 구현한 리졸버는 위와 같이 WebMvcConfigurerAdapter를 구현한 클래스에 Resolver로 등록하면 된다.


근데, 여기서 매개변수(Parameter)와 실행인자(Argument)가 의미하는 게 뭘까?

Parameter(매개변수): 다른 함수에게 전달하는 값 
Argument(실행인자): 실행 시점에서 전달 받는 인자

라고 한다.

이래도 잘 이해가 안되서 찾아보니 지식인에 쉽게 설명해 주신 답글을 발견할 수 있었다.

```c
    int add (int a, int b)
    {
        return a+b;
    }
    /*
        여기서 a와 b는 parameter입니다. 
        함수 정의에서 사용되고 변할 수 없기 때문입니다.
    */

    int main()
    {
        int a = 10;
        int b = 20;
        int c = add (a, b);
        return 0;
    }
    /*
        여기서 a와 b는 argument 입니다. 
        실제로 전달되는 값이고 바뀔 수 있기 때문입닌다. 
        (함수에 a,b 말고 다른 값을 전달 할 수 있다는 뜻)
    */
```

지식인 짱!