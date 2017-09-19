---
title: "Tomcat 설정에 URIEncoding 추가하기"
layout: post
headerImage: false
category: blog
tags:
  - tomcat
  - uriencoding
author: Junyeol Lee
---

%CATALINA_HOME%/conf/server.xml 파일에 아래와 같이 URIEncoding을 추가해 줄 수 있습니다.
요청이 들어올 때, URI인코딩을 UTF-8로 해줍니다.

```
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="utf-8"/>
```


