### <키오스크 Project>

 - 레이아웃 구성
 - 메뉴 버튼 클릭 이벤트
 - 총 금액 띄우기
 - 메뉴 버튼 클릭 시 타임 스탬프 추가
 - 저장하기/불러오기/결제하기 기능
 - 시간 쓰레드 활용하여 띄우기


### Result

![coffeePayment](https://user-images.githubusercontent.com/43839669/101233206-41e23600-36fa-11eb-82ad-21dccae1fece.JPG)


  
  
  
  
### Code

  
```java

import java.awt.BorderLayout;
import java.awt.FlowLayout;
import java.awt.GridLayout;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

/////////////////메뉴 버튼/////////////////////
class MyButton extends JButton {
	private String menuName;
	private int price;

	MyButton(String name, int price) {
		super(name);
		this.menuName = name;
		this.price = price;

	}

	public String getMenuName() {
		return menuName;
	}

	public int getPrice() {
		return price;
	}
}

public class guiProject extends JFrame {
	
	public guiProject() {
		setTitle("Coffee Payment Project");
		setSize(800, 600);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

		///// 패널 생성////

		initNorthPanel();
		add(northPanel, BorderLayout.NORTH);

		initWestPanel();
		add(westPanel, BorderLayout.WEST);

		initCenterPanel();
		add(centerPanel, BorderLayout.CENTER);

		initSouthPanel();
		add(southPanel1, BorderLayout.SOUTH);
		add(southPanel2, BorderLayout.EAST);

		// 기능 버튼 실행
		workButton();
		setVisible(true);
	}

	////////////// NorthPanel->기능버튼/////////////
	private void initNorthPanel() {
		northPanel.setLayout(new FlowLayout(FlowLayout.CENTER, 0, 20));
		northPanel.add(save);
		northPanel.add(load);
		northPanel.add(pay);
	}

	////////////// WestPanel-> 메뉴버튼/////////////
	private void initWestPanel() {
		westPanel.setLayout(new GridLayout(5, 1));
		buttonArr.add(new MyButton("Americano", 4000));
		buttonArr.add(new MyButton("Cafe Latte", 4500));
		buttonArr.add(new MyButton("Cappuccino", 4500));
		buttonArr.add(new MyButton("vanilla Latte", 5000));
		buttonArr.add(new MyButton("caramel Macchiato", 5000));

		for (MyButton b : buttonArr) {
			westPanel.add(b);
			b.addActionListener(e -> {
				String timeStamp = dateFormat.format(new Date());
				textArea.append(b.getMenuName() + " : " + b.getPrice() + "원 " + " " + timeStamp + "\n\n");
				chooseArr.add(b.getMenuName() + ":" + b.getPrice());
				System.out.println(chooseArr);
			});

		}

	}

	////////////// CenterPanel->메뉴 선택 나열/////////////
	private void initCenterPanel() {
		centerPanel.setLayout(new FlowLayout(FlowLayout.LEFT, 20, 20));
		centerPanel.add(textArea);

	}

	////////////// SouthPanel->총 금액&시간/////////////
	private void initSouthPanel() {
		southPanel1.setLayout(new FlowLayout(FlowLayout.LEFT, 50, 20));

		for (MyButton b : buttonArr) {
			b.addActionListener(e -> {
				sum += b.getPrice();
				sumText.setText(Integer.toString(sum) + "원");
			});
		}
		
		southPanel1.add(label);
		southPanel1.add(sumText);
		thread.start();
		//southPanel1.add(timeText);
		southPanel2.setLayout(new FlowLayout(FlowLayout.RIGHT,10,410));
		southPanel2.add(timeText);

	}

	///////////////// 기능 버튼/////////////////////
	private void workButton() {

		/////////// 저장 기능////////////
		save.addActionListener(e -> {
			String timeStamp = fileFormat.format(new Date());
			String fileName = null;
			fileName = "C:\\menu\\" + timeStamp + ".txt";
			try (FileOutputStream out = new FileOutputStream(fileName);
					ObjectOutputStream oOut = new ObjectOutputStream(out);) {
				oOut.writeObject(chooseArr.toString());
			} catch (Exception ex) {
				ex.printStackTrace();
			}

		});

		/////////// 로드 기능////////////
		load.addActionListener(e -> {
			File dir = new File("C:\\menu\\");
			File files[] = dir.listFiles();
			ArrayList<String> arr = new ArrayList<>();
			for (int i = 0; i < files.length; ++i) {
				String fileName = files[i].toString();
				try (FileInputStream fIn = new FileInputStream(fileName);
						ObjectInputStream oIn = new ObjectInputStream(fIn);) {
					try {
						arr.add("\n" + (String) oIn.readObject());
					} catch (ClassNotFoundException ex2) {
						ex2.printStackTrace();
					}

				} catch (Exception e2) {
					e2.printStackTrace();
				}

			}
			JOptionPane.showMessageDialog(null, "<저장 리스트>\n" + arr.toString());
		});

		/////////// 결제 기능////////////

		pay.addActionListener(e -> {
			JOptionPane.showMessageDialog(null, "결제하실 금액은? " + sum + "원");
		});
	}

    ////////////////Time Thread/////////////////
	Thread thread = new Thread(new Runnable() {
		
		@Override
		public void run() {
			while(true) {
				try {
					timeText.setText(timeFormat.format(new Date()));
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
			
		}
	});
	/////////////////// Field/////////////////////

	SimpleDateFormat timeFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
	SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");
	SimpleDateFormat fileFormat = new SimpleDateFormat("yyyyMMdd_HHmmss");

	private JTextArea textArea = new JTextArea(100, 110);

	private JTextField timeText = new JTextField();
	private JTextField sumText = new JTextField();
	private int sum = 0;
	private String timeStamp = null;
	private ArrayList<String> chooseArr = new ArrayList<>();
	private ArrayList<MyButton> buttonArr = new ArrayList<>();
	
	private JLabel label = new JLabel("총 결제액:");
	private JButton save = new JButton("저장하기");
	private JButton load = new JButton("불러오기");
	private JButton pay = new JButton("결제하기");

	private JPanel northPanel = new JPanel();
	private JPanel westPanel = new JPanel();
	private JPanel southPanel1 = new JPanel();
	private JPanel southPanel2 = new JPanel();
	private JPanel centerPanel = new JPanel();

	/////////////////// Main/////////////////////
	public static void main(String[] args) {
		new guiProject();
	}
}

```


