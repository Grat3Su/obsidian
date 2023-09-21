
[[dao]]
- DB 데이터를 받아오기 위해  dao 객체를 생성한다.
``` java
private LoginoutDao loginoutDao = new LoginoutDaoImpl();
```


- 서블릿에서 사용하기 위해 결괏값을 UserDto로 반환해야 한다.
- dao에서 userEmail로 DB에 검색한 유저 정보를 불러온다.
``` java
public UserDto login(String userEmail, String userPassword) {
	UserDto userDto = loginoutDao.login(userEmail);
```


- 회원정보가 존재하고(userDto!=null) DB의 비밀번호와 입력이 같을 때 userdto를 반환한다.
- 이 때 비밀번호는 민감한 정보로 취급해 제외하고 넘겨준다.
- 위의 조건이 아닐 때 null을 넘겨준다.
```java
if(userDto!=null&&userDto.getUserPsssword().equals(userPassword)) {
		userDto.setUserPsssword(null);
		return userDto;
	}
	return null;//useremail이 올바르지 않거나 userpassword가 올바르지 않느 2가지 모두 포함
```


## 전체 코드
``` java
import dto.UserDto;
import loginout.LoginoutDao;
import loginout.LoginoutDaoImpl;

public class LoginoutServiceImpl implements LoginoutService{
private LoginoutDao loginoutDao = new LoginoutDaoImpl();

@Override
public UserDto login(String userEmail, String userPassword) {
	UserDto userDto = loginoutDao.login(userEmail);
	
	if(userDto!=null&&userDto.getUserPsssword().equals(userPassword)) {
		userDto.setUserPsssword(null);
		return userDto;
	}
	return null;//useremail이 올바르지 않거나 userpassword가 올바르지 않느 2가지 모두 포함
}

}

```