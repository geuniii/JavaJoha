## **<학생 관리 프로그램>**

#### *step1.* 다음 학생들의 정보를 Map 에 저장하세요. 

+ __주의)__ Student 클래스를 사용하지 않고 학생 정보도 Map을 사용합니다.
+ (TreeMap<Integer,HashMap<String,Object>>)
+ key 는 학번(Integer)

      {101 : {"name":"홍길동", "average":88, "grade":"B", "contact":"010-2222-1231"},

      102 : {"name":"김길동", "average":92, "grade":"A", "contact":"010-2231-1256"},

      103 : {"name":"김장미", "average":47, "grade":"F", "contact":"010-2512-7754"},

      ...

       }

    ##### <학번 이름 평균 등급 연락처>

    ##### 101 홍길동 88 B 010-2222-1231

    ##### 102 김길동 92 A 010-2231-1256

    ##### 103 김장미 47 F 010-2512-7754

    ##### 201 장아름 85 B 010-9966-3512

    ##### 202 최영수 74 C 010-1111-3864

​


#### *step2.* (1)의 딕셔너리에 학생 3명의 정보를 입력 받아 추가 저장합니다.

- 학생의 이름, 국, 영, 수, 학년, 연락처를 입력 받습니다.

- 학년은 학번의 가장 앞자리에 해당합니다.

  (예를 들어 딕셔너리에 저장된 2학년 학생이 4명이라면, 다음 2학년 학생의 학번은 205가 되어야 합니다.)

  (1 <= 학년 <= 6) 

  (0 <= 학년 당 학생의 수 < 100)



- 평균은 국,영,수 의 평균을 계산하여 저장합니다. (사용자에게 입력 받지 않습니다.)

- 등급은 평균에 따른 A, B, C, D, F 중 하나로 저장합니다. (사용자에게 입력 받지 않습니다.)




     ##### 90 점 이상 : A

     ##### 80점 이상 ~ 90점 미만 : B

     ##### 70점 이상 ~ 80점 미만 : C

     ##### 60점 이상 ~ 70점 미만 : D

     ##### 60점 미만 : F

​

#### *step3.* 사용자 메뉴를 출력합니다. (선택문제)



  ##### 1. 학번으로 검색

  ##### 2. 연락처 뒷번호로 검색

  ##### 3. 1등 학생 보기

  ##### 4. 모든 학생 보기

     ##### 0. 종료



 **1.학번으로 검색**

     학번을 입력 받고 해당 학생의 모든 정보를 출력합니다.

     미등록 학번인 경우 '미등록 학번'을 출력합니다.


 **2. 연락처 뒷번호로 검색**

     연락처 뒷번호 4자리를 입력 받아 연락처가 일치하는

     '모든' 학생들의 이름, 학년, 연락처를 출력합니다. ( 중복 뒷자리도 출력)



 **3. 1등 학생 보기**

     평균을 가지고 1등 학생을 찾아 해당 학생의 학번, 이름, 평균 점수를 출력합니다.

     공동 1등인 경우 학년이 가장 높은 학생을,

     그 중 같은 학년에 공동 1등이 있다면 그 학생들 모두를 출력하세요.



 **4. 모든 학생 보기**

     현재 등록되어있는 모든 학생들의 모든 정보를 출력하세요.

​

```java
import java.util.HashMap;
import java.util.Set;
import java.util.TreeMap;

import javax.swing.JOptionPane;

public class studentManager {

	TreeMap<Integer, HashMap<String, Object>> treeMap = new TreeMap<>();
	Set set = treeMap.keySet();

	////////////////// 학생 추가 메소드/////////////////////
	public void add(int key, String name, int average, String grade, String contact) {
		HashMap<String, Object> hashMap = new HashMap<>();
		hashMap.put("name", name);
		hashMap.put("average", average);
		hashMap.put("grade", grade);
		hashMap.put("contact", contact);
		treeMap.put(key, hashMap);

	}

	///////////////// 학번 생성 메소드/////////////////////
	public int keyMaker(int age) {

		int n = age + 1;
		int count = 0;

		for (Object o : set) {
			int key = Integer.parseInt(o.toString());
			if (key >= age * 100 && key <= n * 100) {
				count += 1;
			}

		}
		int key = age * 100 + count + 1;
		return key;
	}

	///////////////// 등급 계산 메소드/////////////////////
	public String gradeMaker(int average) {
		String grade = null;

		if (average >= 90) {
			grade = "A";
		} else if (average >= 80 && average < 90) {
			grade = "B";
		} else if (average >= 70 && average < 80) {
			grade = "C";
		} else if (average >= 60 && average < 70) {
			grade = "D";
		} else {
			grade = "F";
		}
		return grade;
	}

	///////////////////////////// menu///////////////////////////////////

	private void step2() {

		for (int i = 0; i < 3; ++i) {

			String name = JOptionPane.showInputDialog((i + 1) + "번째 학생 이름을 입력하세요");
			int age = Integer.parseInt(JOptionPane.showInputDialog("학년을 입력하세요"));
			int korean = Integer.parseInt(JOptionPane.showInputDialog("국어 성적을 입력하세요"));
			int english = Integer.parseInt(JOptionPane.showInputDialog("영어 성적을 입력하세요"));
			int math = Integer.parseInt(JOptionPane.showInputDialog("수학 성적을 입력하세요"));
			String contact = JOptionPane.showInputDialog("연락처를 입력하세요");

			// 학번 만들기&성적 구하기
			int key = keyMaker(age);
			int average = (int) (korean + english + math) / 3;
			String grade = gradeMaker(average);

			// 학생 추가
			add(key, name, average, grade, contact);
			JOptionPane.showConfirmDialog(null,
					"이름:" + name + " 학번:" + key + " 평균:" + average + " 등급:" + grade + " 연락처" + contact + "\n등록완료!");
		}

	}

	private void menu1() {

		int searchKey = Integer.parseInt(JOptionPane.showInputDialog(null, "학생의 학번을 입력하세요"));

		for (Object o : set) {
			int key = Integer.parseInt(o.toString());
			if (key == searchKey) {
				JOptionPane.showConfirmDialog(null, key + ":" + treeMap.get(key).toString());
				return;
			} else {
				JOptionPane.showConfirmDialog(null, "미등록 학번입니다!");
				return;
			}

		}

	}

	private void menu2() {

		int count = 0;
		StringBuffer buf = new StringBuffer();

		String searchNum = JOptionPane.showInputDialog(null, "연락처 뒷번호를 입력하세요");

		for (Object o : set) {
			String contact = (String) treeMap.get(o).get("contact");
			String split[] = contact.split("-");
			String lastNum = split[2];
			if (lastNum.equals(searchNum)) {
				count++;
				buf.append(o + ":" + treeMap.get(o) + "\n");
			}

		}
		if (count != 0) {
			JOptionPane.showConfirmDialog(null, buf);
			return;
		} else {
			JOptionPane.showConfirmDialog(null, "미등록 번호입니다!");
			return;
		}
	}

	private void menu3() {

		int max = 0;
		String eliteKey = null;// 1등 key
		StringBuffer buf = new StringBuffer(); // 공동 1등 key
		int count = 1;
		for (Object o : set) {
			String key = o.toString();
			int average = (Integer) treeMap.get(o).get("average");
			// 평균이 1등 점수보다 크다면
			if (average >= max) {
				// 공동 1등
				if (average == max) {
					System.out.println(key.substring(0, 1));
					if (key.substring(0, 1).equals(eliteKey.substring(0, 1))) {
						if (count == 1) {
							buf.append(eliteKey + ":" + treeMap.get(Integer.parseInt(eliteKey)) + "\n");
						}
						// 공동 1등 여러명
						System.out.println("공동 1등 여러명");
						++count;
						buf.append(key + ":" + treeMap.get(Integer.parseInt(key)) + "\n");
						System.out.println(buf);
					} else {
						// 학년 중 1등
						System.out.println("학년 1등");
						eliteKey = key;
						max = average;
						System.out.println(eliteKey + " ");
					}

				} else {

					eliteKey = key;
					max = average;
				}

			}

		}
		if (count != 1) {
			JOptionPane.showConfirmDialog(null, buf);
			return;
		}

		else {
			JOptionPane.showConfirmDialog(null, eliteKey + ":" + treeMap.get(Integer.parseInt(eliteKey)).toString());
		}

	}

	private void menu4() {
		StringBuffer buf = new StringBuffer();
		for (Object o : set) {
			buf.append(o + ":" + treeMap.get(o).toString() + "\n");
		}

		JOptionPane.showConfirmDialog(null, buf.toString());
		return;
	}

	//////////////////////////////////////////////////////////////////////

	public studentManager() {

		// step 1. 다음 학생들의 정보를 Map 에 저장하세요

		add(101, "홍길동", 88, "B", "010-2222-1231");
		add(102, "김길동", 92, "A", "010-2231-1256");
		add(103, "김장미", 47, "F", "010-2512-7754");
		add(201, "장아름", 85, "B", "010-9966-3512");
		add(202, "최영수", 74, "C", "010-1111-3864");

		// menu switch
		String menu;

		menu = "<메뉴선택>\n<*>학생추가<1>학번조회 <2>뒷번호조회\n<3>1등학생조회 <4>모든학생조회 <0>종료";
		String select;
		loop: while (true) {

			select = JOptionPane.showInputDialog(menu);
			switch (select) {

			case "*":
				step2();
				break;

			case "1":
				menu1();
				break;

			case "2":
				menu2();
				break;

			case "3":
				menu3();
				break;

			case "4":
				menu4();
				break;

			case "0":
				JOptionPane.showMessageDialog(null, "종료!");
				break loop;

			}

		}

	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		new studentManager();
	}

}


