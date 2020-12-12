## GUI를 사용하여 homework01번 문제를 Panel 형식으로 구현해보기 

```
  이름 : __________
  레벨 : __________  
        [계산]
  체력 : 
  공격력 : 	
```
->'계산' 버튼 클릭되면 체력, 공격력이 자동으로 하단 label에 출력

### - *RESULT*
![hw03](https://user-images.githubusercontent.com/43839669/101971774-0f46b900-3c77-11eb-88af-b9ef3831837e.jpg)

### - *CODE*

```java
package day35.homework;

import java.awt.Font;
import java.awt.GridLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

public class homework03 extends JFrame {

	private static final String URL = "jdbc:mysql://127.0.0.1/testdb";
	private static final String ID = "testdba";
	private static final String PW = "test1234";

	private Connection connection;
	private PreparedStatement preparedStatement;

	// Label 선언
	private JLabel nameLabel;
	private JLabel levelLabel;
	private JLabel hpLabel;
	private JLabel apLabel;

	// TextField 선언
	private JTextField nameField;
	private JTextField levelField;
	private JTextField hpField;
	private JTextField apField;

	// Button 선언
	private JButton cal = new JButton("체력,공격력 계산하기");
	private JButton save = new JButton("데이터 저장하기");

	// DB 정보
	String name = null;
	int level = 0;
	int hp = 0;
	int ap = 0;

	public homework03() {
		 ////////////////////////GUI 설계//////////////////////////////      
		setTitle("가라 포켓몬!");
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setLayout(new GridLayout(5, 2));
		setFont(new Font("Sans Serif", Font.BOLD, 15));

		// 입력 받기
		nameLabel = new JLabel("<이름>");
		nameLabel.setHorizontalAlignment(JLabel.CENTER);
		nameField = new JTextField();
		levelLabel = new JLabel("<레벨>");
		levelLabel.setHorizontalAlignment(JLabel.CENTER);
		levelField = new JTextField();

		// 체력,공격력
		hpLabel = new JLabel("<체력>");
		hpLabel.setHorizontalAlignment(JLabel.CENTER);
		hpField = new JTextField();
		apLabel = new JLabel("<공격력>");
		apLabel.setHorizontalAlignment(JLabel.CENTER);
		apField = new JTextField();

		 ////////////////////////기능 설계//////////////////////////////
		
		//계산하기 버튼
		cal.addActionListener(e -> {
			if (levelField.getText().isEmpty()) {
				JOptionPane.showMessageDialog(null, "레벨을 입력해주세요!");
			} else {
				level = Integer.parseInt(levelField.getText());
				hp = Integer.parseInt(levelField.getText()) * 100;
				ap = (int) (Integer.parseInt(levelField.getText()) * 0.5);
				hpField.setText(Integer.toString(hp));
				apField.setText(Integer.toString(ap));
			}

		});

		//저장하기 버튼
		save.addActionListener(e -> {
			name = nameField.getText();
			if (hp == 0 || ap == 0) {
				JOptionPane.showMessageDialog(null, "계산하기를 먼저 눌러주세요!");
			} else {
                ////////////////////////DB 연동//////////////////////////////	

				String sql = "INSERT INTO pokemon VALUES" + "(NULL, '" + name + "'," + level
						+ "," + hp + "," + ap+ ",DEFAULT)";		
				
				try {
					connection = DriverManager.getConnection(URL, ID, PW);
					preparedStatement = connection.prepareStatement(sql);
					preparedStatement.execute();
					JOptionPane.showConfirmDialog(null, "쿼리문:" + sql + "성공!");
				} catch (SQLException e1) {
					e1.printStackTrace();
				} finally {

					try {
						if (preparedStatement != null)
							preparedStatement.close();
						if (connection != null)
							connection.close();						
					} catch (SQLException e1) {
						e1.printStackTrace();
					}
				}

				nameField.setText(null);
				levelField.setText(null);
				hpField.setText(null);
				apField.setText(null);
			}			
		});

		add(nameLabel);
		add(nameField);
		add(levelLabel);
		add(levelField);
		add(cal);
		add(save);
		add(hpLabel);
		add(hpField);
		add(apLabel);
		add(apField);
		
		setVisible(true);
	}

	public static void main(String[] args) {
		new homework03();
	}
}
```
