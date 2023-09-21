### 로그인 dao
- DB와 통신하는 역할

1. 필요한 변수 및 객체 선언
2. connection 객체 : DB와 연결하기 위해 사용하는 객체
3. PreparedStatrement :  SQL 구문을 실행하는 역할
4. ResultSet : select문의 결과를 저장하는 역할
``` java
public UserDto login(String userEmail) {
		UserDto userdto = null;
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
```

[[DBManager]]
- DBManager에서 커넥션을 받아옴
``` java
con = DBManager.getConnection();
```


- prepareStatement를 사용하기 위해 SQL 구문을 StringBuilder에 저장
- 복잡한 구문을 쉽게 처리하기 위해서
- 로그인할 email의 userDto 값을 select한다.
[[UserDto]]
``` java
StringBuilder sb = new StringBuilder();
			sb.append("select user_id, user_name, user_email, 
						user_password from users_db ")
						.append("where user_email=?;");
```


- prepareStatement을 세팅한다.
- setString으로 물음표 위치에 값을 넣어준다.
- 1부터 시작한다.
``` java
pstmt = con.prepareStatement(sb.toString());
			pstmt.setString(1, userEmail);
```

- select문을 수행한 결과로 ResultSet 객체의 값을 반환.
- userdto에 값을 저장.
``` java
rs = pstmt.executeQuery();
			if (rs.next()) {
				userdto = new UserDto(rs.getInt("user_id"), 
				rs.getString("user_name"), rs.getString("user_email"),
				rs.getString("user_password"));
			}
```


- 예외가 발생하지 않아도 finally를 통해 커넥션을 반환해준다.
- userdto를 반환한다.
``` java
} catch (Exception e) {
			e.printStackTrace();
		} finally {
			DBManager.releaseConnection(rs, pstmt, con);
		}
		return userdto;
	}
```


## 전체코드
```java
public class LoginoutDaoImpl implements LoginoutDao{

	@Override
	public UserDto login(String userEmail) {
		UserDto userdto = null;
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			// ConnectionPool 로부터 Connection 객체를 얻는다.
			// ConnectionPool 을 먼저 java 실행환경 (Tomcat)으로 부터 이름을 전달하고 그 객체를 얻는다.

			con = DBManager.getConnection();

			// Statement 객체 - select
			StringBuilder sb = new StringBuilder();
			sb.append("select user_id, user_name, user_email, 
						user_password from users_db ")
						.append("where user_email=?;");
			 
			pstmt = con.prepareStatement(sb.toString());
			pstmt.setString(1, userEmail);
			rs = pstmt.executeQuery();
			if (rs.next()) {
				userdto = new UserDto(rs.getInt("user_id"), 
				rs.getString("user_name"), rs.getString("user_email"),
				rs.getString("user_password"));
			}

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			DBManager.releaseConnection(rs, pstmt, con);
		}
		return userdto;
	}

}
```

