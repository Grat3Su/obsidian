
``` java
public class UserDto {
	int userId;
	String userName;
	String userEmail;
	String userPsssword;
	public UserDto(int userId, String userName, String userEmail, String userPsssword) {
		super();
		this.userId = userId;
		this.userName = userName;
		this.userEmail = userEmail;
		this.userPsssword = userPsssword;
	}
	public int getUserId() {
		return userId;
	}
	public void setUserId(int userId) {
		this.userId = userId;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getUserEmail() {
		return userEmail;
	}
	public void setUserEmail(String userEmail) {
		this.userEmail = userEmail;
	}
	public String getUserPsssword() {
		return userPsssword;
	}
	public void setUserPsssword(String userPsssword) {
		this.userPsssword = userPsssword;
	}

}

```