## 포켓몬 관리 프로그램

- **종료**
- **포켓몬 추가**
- **번호로 검색**
- **이름으로 검색**
- **포켓몬 삭제 (이름으로)**
- **레벨업 (이름으로)**
- **모두 레벨업**

### *RESULT*
![hw](https://user-images.githubusercontent.com/43839669/102049690-b583f180-3e24-11eb-8f88-dd89e8f493bb.JPG)



### *CODE*

```java
package day36.basic;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.swing.JOptionPane;

public class Test01 {

	private static final String URL = "jdbc:mysql://127.0.0.1/testdb";
	private static final String ID = "testdba";
	private static final String PW = "test1234";

	private static Connection connection;
	private PreparedStatement preparedStatement;
	private ResultSet resultSet;

	static {
		try {
			connection = DriverManager.getConnection(URL, ID, PW);
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	private String sql;
	private String input;

	//////////////////////// 스트림 닫기/////////////////////////////////
	public void close() {
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

	public void bufferAppend() {
		StringBuffer sb = new StringBuffer();
		try {
			sb.append(resultSet.getInt(1) + ") ");
			sb.append(resultSet.getString(2) + ": ");
			sb.append("레벨: " + resultSet.getInt(3) + " ");
			sb.append("체력: " + resultSet.getInt(4) + " ");
			sb.append("공격력: " + resultSet.getInt(5));
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		JOptionPane.showMessageDialog(null, sb);
	}

	public Test01() {
		String menu = "1. 포켓몬 추가 \n" + "2. 번호로 검색 \n" + "3. 이름으로 검색 \n" + "4. 포켓몬 삭제 \n" // 이름으로 삭제
				+ "5. 레벨업 \n" // 이름으로 레벨업
				+ "6. 모두 레벨업 \n" + "0. 종료";
		menu: while (true) {
			String select = JOptionPane.showInputDialog(menu);
			switch (select) {
			////////////// 0. 종료//////////////
			case "0":
				JOptionPane.showMessageDialog(null, "프로그램을 종료합니다.");
				break menu;
			////////////// 1. 포켓몬 추가//////////////
			case "1":
				sql = "INSERT INTO pokemon(name,level,hp,ap) VALUES (?,?,?,?)";

				String name = JOptionPane.showInputDialog("포켓몬 이름을 입력하세요!");
				int level = Integer.parseInt(JOptionPane.showInputDialog("포켓몬 레벨을 입력하세요!"));
				int hp = level * 100;
				int ap = (int) (level * 0.5);

				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.setString(1, name);
					p.setInt(2, level);
					p.setInt(3, hp);
					p.setInt(4, ap);
					p.execute();
					JOptionPane.showMessageDialog(null, "Insert 성공!");
				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} finally {
					close();
				}
				break;
			////////////// 2. 번호로 검색//////////////
			case "2":
				sql = "SELECT * FROM pokemon WHERE no=?";

				int no = Integer.parseInt(JOptionPane.showInputDialog("검색하실 포켓몬의 번호를 입력하세요"));
				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.setInt(1, no);
					resultSet = p.executeQuery();
					if (resultSet.next()) {
						bufferAppend();
					} else {
						JOptionPane.showMessageDialog(null, "미등록 포켓몬!");
					}

				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} finally {
					close();
				}
				break;
			////////////// 3. 이름으로 검색//////////////
			case "3":

				sql = "SELECT * FROM pokemon WHERE name=?";

				input = JOptionPane.showInputDialog("검색하실 포켓몬의 이름을 입력하세요");
				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.setString(1, input);
					resultSet = p.executeQuery();
					if (resultSet.next()) {
						bufferAppend();
					} else {
						JOptionPane.showMessageDialog(null, "미등록 포켓몬!");
					}
				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} finally {
					close();
				}
				break;

			////////////// "4. 포켓몬 삭제//////////////
			case "4":

				sql = "DELETE FROM pokemon WHERE name=?";

				input = JOptionPane.showInputDialog("삭제하실 포켓몬의 이름을 입력하세요");
				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.setString(1, input);
					p.execute();
					JOptionPane.showMessageDialog(null, "삭제 완료!");
				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} catch (NullPointerException e) {
					JOptionPane.showMessageDialog(null, "미등록 포켓몬!");
				} finally {
					close();
				}
				break;
			////////////// 5. 레벨업//////////////
			case "5":
				sql = "UPDATE pokemon SET level = level+1 WHERE name=?";

				input = JOptionPane.showInputDialog("레벨업 하실 포켓몬의 이름을 입력하세요");
				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.setString(1, input);
					p.execute();
					JOptionPane.showMessageDialog(null, "레벨업 완료!");

				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} catch (NullPointerException e) {
					JOptionPane.showMessageDialog(null, "미등록 포켓몬!");
				} finally {
					close();
				}

				break;

			////////////// 6. 모두 레벨업//////////////
			case "6":
				sql = "UPDATE pokemon SET level = level+1";

				try (PreparedStatement p = (PreparedStatement) connection.prepareStatement(sql)) {
					p.execute();
					JOptionPane.showMessageDialog(null, "모두 레벨업 완료!");

				} catch (SQLException e) {
					e.printStackTrace();
					JOptionPane.showInputDialog("SQL 쿼리문 오류");
				} finally {
					close();
				}
				break;
			default:
				JOptionPane.showMessageDialog(null, "다시 입력하세요.");
				break;
			}
		}
	}

	public static void main(String[] args) {
		new Test01();
	}
}
```
