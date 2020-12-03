### <키오스크 Project>
- 미완성

  *clear*
  - 레이아웃 구성
  - 메뉴 버튼 클릭 이벤트
  - 총 금액 띄우기
  
  *Must*
  - 메뉴 버튼 클릭 시 타임 스탬프 추가
  - 저장하기/불러오기/결제하기 기능
  - 시간 쓰레드 활용하여 띄우기


### Result
  
  
  
```java

import java.awt.BorderLayout;
import java.awt.FlowLayout;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

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
		this.menuName =name;
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
    /////이벤트 리스너/////
	ActionListener listener = new ActionListener() {
		
		@Override
		public void actionPerformed(ActionEvent e) {
			if(e.getSource()==buttonArr) {
				//textArea.append(buttonArr.toString());
				JOptionPane.showMessageDialog(null,"눌렸다!");
			}
		}
	};


	public guiProject() {
		setTitle("Coffee Payment Project");
		setSize(1200, 800);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
		
		/////패널 생성////

		initNorthPanel();
		add(northPanel, BorderLayout.NORTH);
		
		initWestPanel();
		add(westPanel, BorderLayout.WEST);
		
		initCenterPanel();
		add(centerPanel, BorderLayout.CENTER);
		
		initSouthPanel();
		add(southPanel, BorderLayout.SOUTH);		

	}

    //////////////NorthPanel->기능버튼/////////////
	private void initNorthPanel() {
		northPanel.setLayout(new FlowLayout(FlowLayout.CENTER, 0, 20));
		northPanel.add(save);
		northPanel.add(load);
		northPanel.add(pay);
	}

    //////////////WestPanel-> 메뉴버튼/////////////
	private void initWestPanel() {
		westPanel.setLayout(new GridLayout(5,1));
		buttonArr.add( new MyButton("Americano",4000));
		buttonArr.add( new MyButton("Cafe Latte",4500));
		buttonArr.add( new MyButton("Cappuccino",4500));
		buttonArr.add( new MyButton("vanilla Latte",5000));
		buttonArr.add( new MyButton("caramel Macchiato",5000));

		for(MyButton b : buttonArr) {
			
			westPanel.add(b);
			b.addActionListener(e ->{
				textArea.append(b.getMenuName()+" : "+b.getPrice()+"원 \n\n");
				System.out.println(b.getMenuName());
			});
			

		}		

	}
    //////////////CenterPanel->메뉴 선택 나열/////////////
	private void initCenterPanel() {
		centerPanel.setLayout(new FlowLayout(FlowLayout.LEFT,20,20));
		centerPanel.add(textArea);
		
	}
    //////////////SouthPanel->총 금액&시간/////////////
	private void initSouthPanel() {
		southPanel.setLayout(new FlowLayout(FlowLayout.LEFT,50,20));
		southPanel.add(label);
		
	}
	
    ///////////////////Field/////////////////////
	private JTextArea textArea = new JTextArea(100,110);
	
	private JTextField timeText = new JTextField();
	private JTextField sumText = new JTextField();
	
	private ArrayList<MyButton> buttonArr = new ArrayList<>();

	private JLabel label = new JLabel("총 결제액:");
	private JButton save = new JButton("저장하기");
	private JButton load = new JButton("불러오기");
	private JButton pay = new JButton("결제하기");

	private JPanel northPanel = new JPanel();
	private JPanel westPanel = new JPanel();
	private JPanel southPanel = new JPanel();
	private JPanel centerPanel = new JPanel();

    ///////////////////Main/////////////////////
	public static void main(String[] args) {
		new guiProject();
	}
}
```


