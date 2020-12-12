## DB에 저장된 포켓몬 조회하기
- 사용자에게 조회할 포켓몬의 이름을 입력 받기 
- 해당 포켓몬의 정보를 DB에 조회한 뒤 출력
- 조회한 포켓몬이 없으면 '미등록 포켓몬'

### - *RESULT*
![hw02](https://user-images.githubusercontent.com/43839669/101971359-289a3600-3c74-11eb-96c7-68bede33b35c.jpg)


### - *CODE*

```java
package day35.homework;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.swing.JOptionPane;

public class homework02 {

	private static final String URL = "jdbc:mysql://127.0.0.1/testdb";
	private static final String ID = "testdba";
	private static final String PW = "test1234";

	private Connection connection;
	private PreparedStatement preparedStatement;
	private ResultSet resultSet;

	public homework02() {

		loop: while (true) {
			String input = JOptionPane.showInputDialog("조회할 포켓몬 입력! (종료 exit)");
			if (input.equals("exit")) {
				break loop;
			}
			String sql = "SELECT * FROM pokemon WHERE name ='" + input + "'";

			try {
				connection = DriverManager.getConnection(URL, ID, PW);
				preparedStatement = connection.prepareStatement(sql);
				resultSet = preparedStatement.executeQuery();

				if (resultSet.next()) {
					String result = Integer.toString(resultSet.getInt(1)) + ")" + resultSet.getString(2) + " level:"
							+ Integer.toString(resultSet.getInt(3)) + " hp:" + resultSet.getInt(4) + " ap:"
							+ resultSet.getInt(5) + " regDate:" + resultSet.getDate(6);
					JOptionPane.showMessageDialog(null, result);
				} else {
					JOptionPane.showMessageDialog(null, "미등록 포켓몬입니다!");
				}
			} catch (SQLException e) {
				e.printStackTrace();
			} finally {
				try {
					if (resultSet != null)
						resultSet.close();
					if (preparedStatement != null)
						preparedStatement.close();
					if (connection != null)
						connection.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}

	public static void main(String[] args) {
		new homework02();
	}

}

```
