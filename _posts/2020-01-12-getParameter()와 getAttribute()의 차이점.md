---
layout: post
title:  HttpServletRequest의 getParameter와 getAttribute 차이
date:   2020-01-12
author: ash
categories: JSP/Servlet
comments: true
---


## getParameter()와 getAttribute()

HttpServletRequest 객체에 정의되어 있는 메서드이다.

HttpServletRequest는 클라이언트(view단의 JSP 등)에서 요청(request)을 보내면 요청과 관련된 정보를 얻을 수 있는 객체이다.

메서드들이 많이 있기 때문에 안 쓰다보면 까먹는다.

getParameterNames()와 getAttributeNames()가 있는데 

getParameterNames()는 클라이언트에서 보낸 태그의 name 정보를 얻고, getAttributeNames()는 setAttribute로 값을 Key,Value형식으로 담아야 사용가능하며, 반환값도 Object 타입이다.

클라이언트에서 보낸 값을 바로 map에 담아 받으려면 getParameterNames를 사용하면 된다.





##### test.jsp

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
			map.put(attrName, attrValue);					    			//map에 넣는다
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
		사과ㅋ:${apple }<br>
		파인애플ㅋ:${pineapple}<br>
		바나나ㅋ:${banana}
	</div>
</body>
</html>
```


{% if page.comments %}
<div id="post-disqus" class="container">
{% include disqus.html %}
</div>
{% endif %}

