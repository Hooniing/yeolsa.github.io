ModelAndView



RestController 인터페이스를 보면 @ResponseBody 어노테이션이 붙어있다. 이 Response에 내용이 들어간다.



@ResponseStatus(HttpStatus.OK)는 Http 상태값을 Response에 넣어준다. 이 경우는 200 OK



네이밍은 굉장히 중요하다.



@PathVariable안에 패턴(정규 표현식)을 넣을 수 있다.

예) @DeleteMapping("/{id:[\\\d]+]}")
이는 