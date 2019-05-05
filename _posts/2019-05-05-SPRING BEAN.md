---
layout: post
title:  스프링 빈(Spring Bean)
date:   2019-05-05
author: ash
categories: Spring
---

```
참고1 :  https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-extension
참고2 : https://jeong-pro.tistory.com/167 [기본기를 쌓는 정아마추어 코딩블로그]
```

## 1. 스프링 빈?

* * *

**
[spring docs 일부]
In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.
**

- 스프링 빈은 일반적인 자반 객체
- 스프링 IoC 컨테이너에 의해 생성되고 관리되는 자바 객체를 일반적으로 Bean이라고 함
- Bean으로 등록되면 알아서 컨테이너가 인스턴스를 생성
- 자바파일을 만들거나, maven repository를 통해 추가한 라이브러리 등은 Bean으로 등록해줘야 함
- @Component, @Service, @Controller, @Repository, @Bean등의 어노테이션을 생성한 자바객체에 추가해주면, 스프링컨테이너의 Bean으로 등록됨(Component-scan을 통해 scan후, 어노테이션이 붙어있는 객체들을 Bean으로 생성한다)
- 등록된 Bean은 @Autowired 등의 어노테이션으로 주입받아서 사용이 가능함
