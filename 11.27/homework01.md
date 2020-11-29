# <Callback 함수>
#### 특정 함수에 매개변수로 전달된 함수

- callback을 넘겨 받는 함수 내에서 Callback 함수를 실행할 수 있다.
- 비동기식 처리를 (Asynchronous) 요청했던 일이 끝난 후 처리 결과를 Callback 함수와 함께 전달할 수 있다.

#### [Example]

- 마스터가 10명의 슬레이브들에게 일을 시킨다.
- 슬레이브들 각자가 일한 시간을 마스터의 MasterCalculator 인터페이스의 printWage()함수를 호출해 자신의 급여를 계산한다.

**=>마스터가 일일이 슬레이브의 일한 시간을 추적하고 확인하는 연산의 수고를 덜 수 있다.**

```java
public class Master {
	interface MasterCalculator {
		void printWage(int time, int wage);
	}
	
	public static void main(String[] args) {
		MasterCalculator cal = new MasterCalculator() {
			@Override
			public void printWage(int time, int wage) {
				System.out.println("일한 시간 : " + time + ", 급여 : $" + time * 9);
			}
		};
		
		Thread[] slave = new Thread[10];
		for(int i=0; i<10; i++) {
			try {
				slave[i] = new Thread(new Slave(cal));
				System.out.print("Slave " + (i+1) + " >> ");
				slave[i].start();
				Thread.sleep(((int)(Math.random() * 4) + 1) * 1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}

class Slave implements Runnable{
	Master.MasterCalculator mCal;
	
	Slave(Master.MasterCalculator mCal){
		this.mCal = mCal;
	}
	
	private int calWage(int time) {
		return time * 9;
	}
	
	@Override
	public void run() {
		try {
			int time = (int)(Math.random() * 24) + 1;
			Thread.sleep(time);
			mCal.printWage(time, calWage(time));
		} catch(InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```
#### [Console]
```
Slave 1 >> 일한 시간 : 5, 급여 : $45
Slave 2 >> 일한 시간 : 13, 급여 : $117
Slave 3 >> 일한 시간 : 4, 급여 : $36
Slave 4 >> 일한 시간 : 24, 급여 : $216
Slave 5 >> 일한 시간 : 16, 급여 : $144
Slave 6 >> 일한 시간 : 8, 급여 : $72
Slave 7 >> 일한 시간 : 18, 급여 : $162
Slave 8 >> 일한 시간 : 19, 급여 : $171
Slave 9 >> 일한 시간 : 23, 급여 : $207
Slave 10 >> 일한 시간 : 22, 급여 : $198
