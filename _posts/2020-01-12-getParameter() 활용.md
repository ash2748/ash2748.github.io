---
layout: post
title:  getParameter() 활용
date:   2020-01-12
author: ash
categories: JSP/Servlet
comments: true
---

회사 프로젝트에서 DB값을 전달하고 전달 받기 위해 MAP공통 프레임워크 만들어둔 것을 사용하는데, 굉장히 오래된 소스이다.
DB의 데이터를 주고 받을 때, 보통 DTO나 MAP을 사용하는 것 같은데 지금 프로젝트는 MAP방식으로 사용하고 있다.
그래서 @RequestMapping 등의 어노테이션도 안 쓰고 DTO도 따로 안만들어 사용하고 있다.
어느 것을 쓰는게 좋다는 논쟁이 많은 것 같은데, 내공이 부족하여 어느 것이 장점이 많은지 확실히는 모르겠다.
그냥 초심자 입장에서 쓰는 입장에서만 보면 map이 편한 것 같긴 한데 찾아보니 객체지향적으로 볼 때는 DTO가 더 좋다고 하는 것 같다.
나도 공통MAP 프레임워크 같은 것을 만들어보면 도움이 될 것 같아서 한 번 만들어보려는데 오랜만에 해보려니 잘 안된다.
지금 포스팅은 DB까지의 데이터 전송은 아니더라도, HttpServletRequest 객체 쓰임법부터 차근차근 공부하며 포스팅 하려고 한다.


## getParameter()

HttpServletRequest 객체에 정의되어 있는 메서드이다.
HttpServletRequest는 클라이언트(view단의 JSP 등)에서 요청(request)을 보내면 요청과 관련된 정보를 얻을 수 있는 객체이다.
요청 정보를 하나하나 받으려면 getParameter(클라이언트의 태그 name)을 사용하면 된다.
이 요청값들을 한꺼번에 받기 위해선 getParameterNames()을 활용하면 된다.
getParameterNames()는 클라이언트에서 보낸 태그의 name 정보들을 담고 있다.
클라이언트에서 보낸 값을 바로 map에 담아 받으려면 getParameterNames()를 사용하면 된다.


##### requestView.jsp

``` html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
<form action="/test">
	<input type="text" name="apple">
	<input type="text" name="pineapple">
	<input type="text" name="banana">
	<input type="submit" alt="버튼">
</form>
</body>
</html>
```



###### HomeController.java

```java

@Controller
public class HomeController {
	@RequestMapping(value="/view", method = RequestMethod.GET)
	public String test(Map<String, Object> map,HttpServletRequest request) {
		return "/requestView";
	}

	@RequestMapping(value="/requestResult", method = RequestMethod.GET)
	public String test2(Map<String, Object> map, HttpServletRequest request) {
		Enumeration<String> attrNames = request.getParameterNames();	//클라이언트(JSP)에서 요청 값들의 이름을 담는다
		  

		while(attrNames.hasMoreElements()) {							  //attNames의 값이 없을 때까지 반복
			String attrName =attrNames.nextElement();					//attrName의 요소를 담는다(요청값의 name)
			String attrValue =request.getParameter(attrName);	//name을 파라미터 인자로 넣어 view에서 보낸 value값을 얻는다
			map.put(attrName, attrValue);					    			  //map에 넣는다
		}
	
		return "/requestResult";
	}
}

```



##### requestResult.jsp

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<div>
		사과:${apple}<br>
		파인애플:${pineapple}<br>
		바나나:${banana}
	</div>
</body>
</html>
```

오랜만에 써서 getAttribute()랑 활용법을 헷깔렸다.
getAttribute(),getAttributeNames()는 setAttribute(key,value)를 셋팅해줘야 사용 가능하다.
단순히 클라이언트 요청값을 주고받을 때에는 getParameter(),getParameterNames()를 활용하면 된다.



{% if page.comments %}
<div id="post-disqus" class="container">
{% include disqus.html %}
</div>
{% endif %}

