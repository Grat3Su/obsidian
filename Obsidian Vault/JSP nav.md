

1. nav 파일을 생성하고
``` jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%!
	String str = "ssafy";
%>    
    <nav>
	<a href = "naver.com"> naver</a>
	<a href = "google.com"> google</a>
    <%=str %>
    </nav>
```

2. 사용하고자 하는 파일에 include 한다
``` jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%!
String str = "QQQQ";
%>
<jsp:include page="nav.jsp" flush="true"/>
	<h2>Body2</h2>
	<%= str %>
</body>
</html>
```
