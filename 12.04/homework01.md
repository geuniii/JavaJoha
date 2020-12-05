## "GO TO GOAL!"



- Result
![GoalGame](https://user-images.githubusercontent.com/43839669/101233279-0431dd00-36fb-11eb-8c79-2e31f1e953b2.JPG)


- Code

```java
import java.awt.Color;
import java.awt.GridLayout;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.ArrayList;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
// button 을 8*8 만들어서 빠져 나가는 게임
public class ButtonGame extends JFrame {

	/////////////////버튼 class////////////////////
	static class MyButton extends JButton {

		private int status;
		public int getStatus() {
			return status;
		}

		public void setColor(int status) {
			switch (status) {
			case CURRENT:
				this.setBackground(Color.RED);
				break;
			case ROAD:
				this.setBackground(Color.WHITE);
				break;
			case WALL:
				this.setBackground(Color.BLACK);
				break;
			case END:
				this.setBackground(Color.BLUE);
				break;

			}
		}
        ///버튼 생성자///
		public MyButton(int status) {

			super();
			this.status = status;
			this.setColor(status);
		}
	}
    ///////////////////Field///////////////////////
	private static final int ROAD = 0;
	private static final int WALL = 1;
	private static final int END = 2;
	private static final int CURRENT = 3;
	private int x, y = 0; //현재 위치
	private int tempX, tempY = 0; //이전 위치

	private static final int[][] map = {
			{ CURRENT, ROAD, WALL, WALL, WALL, ROAD, WALL, ROAD },
			{ ROAD, ROAD, ROAD, ROAD, WALL, ROAD, WALL, ROAD }, 
			{ WALL, WALL, WALL, ROAD, ROAD, ROAD, ROAD, ROAD },
			{ WALL, ROAD, ROAD, ROAD, WALL, ROAD, WALL, ROAD }, 
			{ WALL, WALL, WALL, ROAD, WALL, WALL, WALL, ROAD },
			{ ROAD, ROAD, ROAD, ROAD, ROAD, ROAD, WALL, END }, 
			{ ROAD, WALL, WALL, WALL, WALL, WALL, WALL, ROAD },
			{ ROAD, ROAD, ROAD, ROAD, ROAD, ROAD, ROAD, ROAD }

	};

	private MyButton[][] buttonArr = new MyButton[8][8];
	private JButton curBtn = null; //현재 버튼
	
	/////////////////버튼 생성////////////////////
	public void buttonMaker() {
		for (int i = 0; i < 8; ++i) {
			for (int j = 0; j < 8; ++j) {
				buttonArr[i][j] = new MyButton(map[i][j]);
			}
		}
	}

	////////////////프레임 구성//////////////////
	public ButtonGame() {
		buttonMaker();
		setTitle("GO TO GOAL!");
		setSize(800, 800);
		setLocationRelativeTo(null);
		setLayout(new GridLayout(8, 8));
		setDefaultCloseOperation(EXIT_ON_CLOSE);

		for (MyButton[] b1 : buttonArr) {
			for (MyButton b2 : b1) {
				this.add(b2);
			}
		}
     ////////////////Key Event//////////////////
		curBtn = buttonArr[x][y]; //현재 위치 버튼
		curBtn.addKeyListener(new KeyListener() {

			@Override
			public void keyTyped(KeyEvent e) {
			}

			@Override
			public void keyReleased(KeyEvent e) {
			}

			@Override
			public void keyPressed(KeyEvent e) {
				//이전 위치
				tempX = x; 
				tempY = y;
				switch (e.getKeyCode()) {
				case KeyEvent.VK_UP:
					if (x != 0 && map[x - 1][y] != 1) {
						x -= 1;
					}
					break;
				case KeyEvent.VK_DOWN:
					if (x != 7 && map[x + 1][y] != 1) {
						x += 1;
					}
					break;
				case KeyEvent.VK_LEFT:
					if (y != 0 && map[x][y - 1] != 1) {
						y -= 1;
					}
					break;
				case KeyEvent.VK_RIGHT:
					if (y != 7 && map[x][y + 1] != 1) {
						y += 1;
					}

					break;

				}
				//////////////////이전 버튼 색 변환/////////////////////
				if (buttonArr[tempX][tempY] == buttonArr[0][0]) {
					buttonArr[tempX][tempY].setBackground(Color.WHITE);
				} else {
					buttonArr[tempX][tempY].setColor(map[tempX][tempY]);
				}
                //////////////////바뀐 버튼 색 변환/////////////////////
				curBtn = buttonArr[x][y];
				if (buttonArr[x][y].getBackground() == Color.BLUE) {
					JOptionPane.showMessageDialog(null, "탈출 성공!");
				}
				curBtn.setBackground(Color.RED);

			}
		});

		setVisible(true);

	}

	public static void main(String[] args) {
		new ButtonGame();
	}
}

```
