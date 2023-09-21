## 전체 코드

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
	<h1>로그인</h1>
	<form action="/Loginout" method="post">
		
		<input type="hidden" name="job" value ="login"><br>
		<input type="text" name="userEmail"><br>
		<input type="text" name="userPassword"><br>
		<input type="submit" value="로그인">
	</form>

	<h1>ajax 로그인</h1>
	<form action="/Loginout">
	<!-- 디자인이 깨지면 form 태그만 유지 -->
		<input type="hidden" name="job" value ="login"><br>
		<input id = "userEmail" type="text" name="userEmail" value ="dong@gmail"><br>
		<input id = "userPassword" type="text" name="userPassword" value = "1234"><br>
	</form>
		<input id = "btnLogin" type="button" value="로그인">
		<!-- 전송버트는 form 바깥으로 type = button -->
<script>
    window.onload = function(){
        document.querySelector("#btnLogin").onclick = login;
    }

    async function login(){
        // 사용자가 입력한 파라미터 준비
        // 백엔드 url 요청 - fetch post
        // 결과 : json
        // 성공 / 실패
        let userEmail = document.querySelector("#userEmail").value;
        let userPassword = document.querySelector("#userPassword").value;

        let urlParams = new URLSearchParams({
            job: 'login_ajax',
            userEmail: userEmail,
            userPassword: userPassword
        });

        let fetchOptions = {
            method: "POST",
            body: urlParams
        }
        let response = await fetch("/basic/Loginout", fetchOptions);
        let data = await response.json();
        console.log(data);
        
        if(data.result == "success"){
        	window.location.href = "/basic/main"
        } else if(data.result == "fail"){
        	alert("아이디 또는 비밀번호가 틀립니다.");
        }
    }
</script>
</body>
</html>
```