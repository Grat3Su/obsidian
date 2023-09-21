[[구조]]
- 서블릿을 사용할 경로 지정
- 함수 여러 개가 서비스를 사용하기 때문에 전역 객체로 생성한다.
[[service]]
``` java
@WebServlet("/Loginout")
public class LoginoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private LoginoutService loginoutService = new LoginoutServiceImpl();
```

- Get Post : Http 메소드로 클라이언트에서 서버로 요청할 때 사용한다.
- Post로 받아서 Get이 처리했다.
- 받은 요청 job에 따라 case문으로 나누었다.
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String job = request.getParameter("job");
		switch (job) {
		case "login": login(request, response);	break;
		case "logout": logout(request, response);	break;
		case "login_ajax": loginAjax(request, response);	break;

		}
	}
	
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
```


### 로그인 함수
1. 동기 방식
- email과 pw를 요청에서 받아와서 service의 로그인 함수에 전달한다.
```java
private void login(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("login");
		String userEmail = request.getParameter("userEmail");
		String userPassword = request.getParameter("userPassword");		
//DB 로그인 성공
//사용자 정보를 DB에서 가져와서 UserDto 객체를 만든 후 session에 저장해서 사용
		UserDto userDto =loginoutService.login(userEmail, userPassword);
```

- 서비스 login 함수가 리턴한 userdto가 존재한다면 세션에 저장한다.
- 요청에 대한 답(페이지 이동 url)을  forward 방식으로 건네준다.
``` java
//성공
if(userDto!=null) {
	request.getSession().setAttribute("userDto", userDto);
	request.getRequestDispatcher("/main").forward(request, response);
} else {			request.getRequestDispatcher("/login/loginFail.jsp").forward(request, response);
}
```

2. 비동기 방식
- 여기까지는 동기 방식과 같다.
``` java
private void loginAjax(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    System.out.println("loginAjax");
	    String userEmail = request.getParameter("userEmail");
	    String userPassword = request.getParameter("userPassword");
	    
	    UserDto userDto = loginoutService.login(userEmail, userPassword);
	    System.out.println(userDto);
```


Gson? : json 구조를 띈 직렬화된 데이터를 자바 객체로 변환해주는  자바 라이브러리
[[기타]]
객체가 존재할 때(회원 정보가 있을 때) 세션에 로그인 정보를 저장하고 json 객체에 담는다.
담은 json 객체를 str로 변환하고 Http 응답을 반환한다.
``` java
Gson gson = new Gson();
	    JsonObject jsonObject = new JsonObject();
	    
	    if(userDto!=null) {
	    	request.getSession().setAttribute("userDto", userDto);
	    	jsonObject.addProperty("result", "success");
	    }else {
	    	jsonObject.addProperty("result", "fail");	    	
	    }
	    String jsonStr = gson.toJson(jsonObject);	    
	    response.getWriter().write(jsonStr);	    
	}
```


### 로그아웃 함수
- 세션을 초기화하고 forward 방식으로 url을 전달한다
``` java
private void logout(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("logout");
		request.getSession().invalidate();//세션 무효
		
		//화면
		request.getRequestDispatcher("/main").forward(request, response);
	}
```


## 전체 코드
``` java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.google.gson.Gson;
import com.google.gson.JsonObject;

import dto.UserDto;
import jdk.nashorn.internal.runtime.options.LoggingOption;
import main.service.LoginoutService;
import main.service.LoginoutServiceImpl;

/**
 * Servlet implementation class Loginout
 */
@WebServlet("/Loginout")
public class LoginoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private LoginoutService loginoutService = new LoginoutServiceImpl();

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String job = request.getParameter("job");
		switch (job) {
		case "login": login(request, response);	break;
		case "logout": logout(request, response);	break;
		case "login_ajax": loginAjax(request, response);	break;

		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
	
	private void login(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("login");
		String userEmail = request.getParameter("userEmail");
		String userPassword = request.getParameter("userPassword");
		System.out.println(userEmail);
		System.out.println(userPassword);
		
		//DB 로그인 성공
		//사용자 정보를 DB에서 가져와서 UserDto 객체를 만든 후 session에 저장해서 사용
		UserDto userDto =loginoutService.login(userEmail, userPassword);
		
		//성공
		if(userDto!=null) {
			request.getSession().setAttribute("userDto", userDto);
			request.getRequestDispatcher("/main").forward(request, response);
		} else {
			request.getRequestDispatcher("/login/loginFail.jsp").forward(request, response);
		}
		
		//세션에 로그인한 사용자 정보를 저장
		//null이면 로그인 실패
//		request.getSession().setAttribute("userDto", userDto);
		
		//화면
//		request.getRequestDispatcher("/main").forward(request, response);
	}
		

	private void logout(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("logout");
		request.getSession().invalidate();//세션 무효
		
		//화면
		request.getRequestDispatcher("/main").forward(request, response);
	}
	
	private void loginAjax(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    System.out.println("loginAjax");
	    String userEmail = request.getParameter("userEmail");
	    String userPassword = request.getParameter("userPassword");
	    System.out.println(userEmail);
	    System.out.println(userPassword);
	    
	    // DB 로그인 성공
	    // 사용자정보를 DB 에석 가져와서 UserDto 객체를 만든 후 session 에 저장
	    UserDto userDto = loginoutService.login(userEmail, userPassword);
	    System.out.println(userDto);
	    
	    Gson gson = new Gson();
	    JsonObject jsonObject = new JsonObject();
	    
	    if(userDto!=null) {
	    	request.getSession().setAttribute("userDto", userDto);
	    	jsonObject.addProperty("result", "success");
	    }else {
	    	jsonObject.addProperty("result", "fail");
	    	
	    }
	    String jsonStr = gson.toJson(jsonObject);
	    System.out.println(jsonStr);
	    
	    response.getWriter().write(jsonStr);
	    
	}
}

```