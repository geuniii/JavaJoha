## *Quiz01*
##### 2단.txt ~ 9단.txt 구구단 텍스트 파일 생성

```java

import java.io.FileWriter;
import java.io.IOException;

public class Quiz01 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String fileName = null;
	    FileWriter writer = null;
	
		try {
			for(int i = 2; i<10; ++i) {
				fileName = String.valueOf(i)+"단.txt";
				writer = new FileWriter(fileName);								
				for(int j = 1; j<10; ++j) {
					writer.write(i+"X"+j+"="+i*j+"\n");
				}
				writer.close();
			}
			System.out.println("완료!");
			
			
		}catch(IOException e) {
			e.printStackTrace();
		}

		
	}

}

```

## *Quiz02*
#####  사용자에게 '단'을 입력 받아 해당 구구단 txt 파일을 열고, 그 내용을 jop으로 출력 

```java
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;
import java.util.Scanner;

import javax.swing.JOptionPane;

public class Quiz02 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		StringBuffer buf = new StringBuffer();
		Scanner sc = null;
		String fileName = null;
		FileReader reader = null;
		try {
				String number =JOptionPane.showInputDialog("원하시는 단을 입력하세요!");
				
				fileName = number + "단.txt";
				reader = new FileReader(fileName);
				sc = new Scanner(reader);
				
				while(sc.hasNext()) {
			
					buf.append(sc.next()+"\n");
	
				}
				
				JOptionPane.showMessageDialog(null,buf);
			
	}
		
		catch(FileNotFoundException e) {
			e.printStackTrace();
		}
		
		finally {
			try {
				if(sc!=null) {
					sc.close();
				}
				if(reader !=null) {
					reader.close();
				}
				
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}

	}
}
```

## *Quiz03*
##### 사용자에게 exit를 입력 받을 때까지 모든 문자열을 jop으로 입력 받고  exit를 입력 하면 YYYYMMdd_HHmmss.txt 형식으로 log 파일을 생성하여 사용자가 입력한 모든 문자열 저장


```java

import java.io.FileWriter;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

import javax.swing.JOptionPane;
public class Quiz03 {
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String fileName = null;
		FileWriter writer  = null;
		try {
			SimpleDateFormat format = new SimpleDateFormat ( "yyyyMMdd_HHmmss");
			Date date = new Date();
			String dateString =format.format(date);
			fileName =dateString+".txt";
			
			StringBuffer buf = new StringBuffer();
			writer= new FileWriter(fileName);
			while(true) {
				
				String string = JOptionPane.showInputDialog("아무 말이나 쓰시고 Exit를 쓰세요!");	
				
				if("exit".equalsIgnoreCase(string)) {	
					writer.write(buf.toString());
				
					break;
				}
				
				
					buf.append(string);
				
				
			}					
			
		}catch(IOException e) {
			
			e.printStackTrace();
		}
		finally {
			try {
				writer.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		
	}

}
```

## *Quiz01-2*
##### Quiz01 쓰레드로

```java
import java.io.FileWriter;
import java.io.IOException;

class GugudanOutThread extends Thread {
	private int dan;
	String fileName = null;
	FileWriter writer = null;

	public GugudanOutThread(int dan) {
		// TODO Auto-generated constructor stub
		this.setDan(dan+1);
	}

	public int getDan() {
		return dan;
	}

	public void setDan(int dan) {
		this.dan = dan;
	}

	@Override
	public void run() {

		try {
			fileName = String.valueOf(getDan()) + "단.txt";
			writer = new FileWriter(fileName);

			for (int i = 1; i < 10; ++i)
				writer.write(getDan() + "X" + i + "=" + getDan()* i + "\n");

			System.out.println("완료!");

		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				writer.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}

		}
	}

}

class Quiz01_2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		GugudanOutThread[] threads = new GugudanOutThread[9];
		for(int i = 1; i<9;++i) {
			threads[i]=new GugudanOutThread(i);
			threads[i].start();
		}
	}

}

