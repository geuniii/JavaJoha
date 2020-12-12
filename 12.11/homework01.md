## 포켓몬 정보 DB저장
- Scanner(Jop) 를 사용하여 새 포켓몬의 이름, 레벨을 입력 받는다.
- 공격력은 레벨의 0.5 배
- 체력은 레벨의 100 배로 세팅하여 이를 DB에 저장 (INSERT)

### - *RESULT*
![hw01](https://user-images.githubusercontent.com/43839669/101971134-672ef100-3c72-11eb-8e66-3967e782203f.jpg)


### - *CODE*

```java
package day35.homework;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.swing.JOptionPane;

public class homework01 {
	private static final String URL = "jdbc:mysql://127.0.0.1/testdb";
	private static final String ID = "testdba";
	private static final String PW = "test1234";
	

	private Connection connection;
	private PreparedStatement preparedStatement;

	public homework01() {

		String name = JOptionPane.showInputDialog("새 포켓몬의 이름은?!");
		String level = JOptionPane.showInputDialog(name + "의 레벨은?!");
		int ap = (int)(Integer.parseInt(level) * 0.5);
		int hp = Integer.parseInt(level) * 100;
		
		try {
			connection = DriverManager.getConnection(URL, ID, PW);
			String sql = "INSERT INTO pokemon VALUES"
					+ "(NULL, '"+name+"',"+Integer.parseInt(level)+","+hp+","+ap+",DEFAULT)";
			preparedStatement = connection.prepareStatement(sql);
			preparedStatement.execute();
			
			JOptionPane.showConfirmDialog(null,"쿼리문:"+sql+"성공!");
			
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {
				if (preparedStatement != null)
					preparedStatement.close();

				if (connection != null)
					connection.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}

		}
		
	}

	public static void main(String[] args) {

		new homework01();

	}

}
```

