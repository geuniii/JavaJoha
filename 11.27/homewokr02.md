# <자판기 구현하기> 
**- 동기 (synchronized) 사용** 

 **1)** 자판기는 2개가 있다.

 **2)** 사람쓰레드는 10개가 있다.

 **3)** 자판기에서 음료를 뽑는 시간은 3초다.

 **4)** 사람쓰레드는 1번 자판기를 우선적으로 선택,

     자판기가 사용중이라면 2번 자판기를 사용한다.

     2번 자판기도 사용중이라면 1번 자판기로 가서 기다린다.
     
     +1, 2 번 자판기 중 먼저 사용가능한 자판기를 선택하여 사용한다.

 **ex)** 사람 1이 사용중입니다/ 사용 끝났습니다 sysout
 
 ### [실패,,] 
 ```java
 
import java.util.ArrayList;

///////////////1번 기계 class////////////////
class Machine01 {
	synchronized static void machine(String name) {
		System.out.println(name + "이 1번 자판기 사용중!");
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println(name + "이 1번 자판기 사용끝!");
	}
}

///////////////2번 기계 class////////////////
class Machine02 {
	synchronized static void machine(String name) {
		System.out.println(name + "이 2번 자판기 사용중!");
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println(name + "이 2번 자판기 사용끝!");
	}
}

/////////////User Thread class////////////////
class User extends Thread {
	public void run() {
		Machine01.machine(getName());
	}
}

/*
 * class User2 extends Thread { public void run() {
 * Machine02.machine(getName()); } }
 */

/////////////vendingMachineProject//////////////
public class vendingMachineProject {

	ArrayList<User> array = new ArrayList<>();

	// start
	void priorMaker() {
		int prior = 10;
		Boolean bool = true;
		for (int i = 0; i < 10; i++) {

			array.add(new User());
			array.get(i).setName(Integer.toString(i) + "번째 손님");
			array.get(i).setPriority(prior);
			--prior;
			if (prior == 0) {
				start();
			}
		}

		if (!bool) {
			start();
		}

	}

	void start() {
		for (int j = 0; j < 9; j++) {
			array.get(j).start();
		}
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		vendingMachineProject v = new vendingMachineProject();
		v.priorMaker();
	}

}
```

### [console]
```
0번째 손님이 1번 자판기 사용중!
0번째 손님이 1번 자판기 사용끝!
9번째 손님이 1번 자판기 사용중!
9번째 손님이 1번 자판기 사용끝!
8번째 손님이 1번 자판기 사용중!
8번째 손님이 1번 자판기 사용끝!
7번째 손님이 1번 자판기 사용중!
7번째 손님이 1번 자판기 사용끝!
6번째 손님이 1번 자판기 사용중!
6번째 손님이 1번 자판기 사용끝!
5번째 손님이 1번 자판기 사용중!
5번째 손님이 1번 자판기 사용끝!
4번째 손님이 1번 자판기 사용중!
4번째 손님이 1번 자판기 사용끝!
1번째 손님이 1번 자판기 사용중!
1번째 손님이 1번 자판기 사용끝!
2번째 손님이 1번 자판기 사용중!
2번째 손님이 1번 자판기 사용끝!
3번째 손님이 1번 자판기 사용중!
3번째 손님이 1번 자판기 사용끝!
