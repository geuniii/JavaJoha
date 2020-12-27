# WORD GEUNIUS

영어 단어 암기를 게임을 통해 흥미롭고 효율적으로 학습할 수 있는 단어 맞추기 게임이다.
최고 기록을 갱신하며 타 사용자들과의 경쟁을 통해 학습 의욕을 상승시킬 수 있다.

사용한 api,ide버전
jdk 15.0.1
ide : eclipse
DB : mysql  Ver 8.0.20, MariaDB 10.5.8

주요 기능
- 로그인, 회원가입
- DB에 있는 단어 랜덤 추출, 그래픽화
- 단어 좌/우 랜덤하게 이동
- 단어 생성, 이동 속도 조절
- 단어 뜻 정답 체크
- 상위 점수 아이디 랭킹 나열
- 사용자 랭킹 조회

작품의 특장점
- 멀티 스레딩으로 다양한 기능을 병렬적, 경제적 수행
  * 진행된 정도 표시 스레드 / 정답 속도 타이머 스레드 /단어 생성 스레드 / 그래픽 repaint 스레드
  
![스레드 구성](https://user-images.githubusercontent.com/62981623/103164973-427f6f80-4855-11eb-8c0d-888fbf562e73.jpg)

- 더블 버퍼링으로 그래픽의 움직임을 부드럽게 표현
  * 보이지 않는 화면을 하나 추가하여 버퍼 역할을 해주어 단어의 움직임을 끊김 없이 표현한다.
```java
  		Image offScreenImage = getParent().createImage(getSize().width, getSize().height);
		Graphics offScreen = offScreenImage.getGraphics();
```

- 관계형 데이터베이스 활용을 통해 순위 조회, 기록 갱신
  * 레벨 별로 총 인원과, 사용자의 순위 조회로 랭킹을 보여주며, 최고 기록을 넘을 시 갱신한다.
```java
	public StringBuffer myRankSearch(String id, int rankNum) {

		Connection conn = null;
		PreparedStatement p = null;
		ResultSet rs = null;

		String sql = "SELECT ID, rank() over(order by max_score DESC) from maxscore where level=?";

		StringBuffer buf = new StringBuffer("<YOUR RANK>\n");
		
		String[] countArr = userCount();
		String[] levelArr = new String[rankNum];
		String[] rankArr = new String[rankNum];

		for (int i = 0; i <rankNum; i++) {
			int searchLevel = i + 1;
			levelArr[i] = String.valueOf(searchLevel);
			try {
				p = (conn = getConnection()).prepareStatement(sql);
				p.setInt(1, searchLevel);
				rs = p.executeQuery();
				while (rs.next()) {					
					if (rs.getString(1).equals(id)) {						
						rankArr[i] = String.valueOf(rs.getString(2));
						break;
					} else {
						rankArr[i] = "X ";
					}
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}

			buf.append("level" + levelArr[i] + ":   " + rankArr[i] + "등 / " + countArr[i] + "명 \n");
		}
		close(conn, p, rs);
		return buf;
	}
```

문제점 및 해결방안 

1. 스윙 이벤트 스레드의 중단 문제  
게임 동작 화면에서 스윙 관련 여러 스레드가 동시에 하나의 스윙 컴포넌트에 접근해서 데이터를 조작하다보니  
충돌 상황이 일어나 모든 스레드가 중단되는 어려움이 있었다.  
--> invokeLater()을 사용하여 이벤트 처리 쓰레드로 이벤트들을 큐에 넣어 진행하고 있는 작업과 분리해서 실행시켜 해결하였다.  

2. 단어 : 뜻이 1 : n 관계일 경우 다중 정답 처리 문제  
사용자가 적은 답을 정답과 비교하려면 1:1 관계가 되어야 하는데, 다양한 답이 정답으로 처리될 수 있어야 하므로 어려움을 겪었다.  
- 첫번째 시도
```sql
meaning LIKE '%answer%' 
```



2. Panel 에 더블 버퍼링으로 그래픽스 구현  
더블 버퍼링을 구현하기 위하여 화면의 크기를 파라미터로 넘겨 주어야 보이지 않는 화면을 생성할 수 있는데,  
Panel의 크기를 파라미터로 넘겨주는데에 어려움을 겪었다.

단어가 생성되고, 움직일 그래픽을 Frame 전체가 아닌, 일부 panel에서 동작하는데에 어려움이 있었다. 
- BGM으로 넣었던 음악이 frame 생성시마다 여러번 호출되는 문제
- 단어: 뜻 이 1:n 관계일 경우 다중 정답 문제

개선방안
- socket 통신을 통해 대결 기능 추가
- 틀린 단어 단어장 추가

1) 이클립스에서 프로젝트를 깃에 바로 올리는 방법 
  2) readme.md 추가 (도큐먼트 링크, 시연 동영상 유튜브 링크) 
	ㄱ. 프로젝트 소개 (메인 주제) 
	ㄷ. 사용한 api, ide 등의 버전 
		예) 
			JDK 1.8
			IDE : 이클립스
			Database : mysql 15.1, MariaDB 10.5.8
					(cmd $mysql --version)
			외부 라이브러리 : ~~~~
	ㄴ. 잘한 점 
		- 어떤 기능에 포커스를 두었는지 
		- 기능 구현을 위해 어떤 새로운 공부를 했는지
		- 설계를 어떻게 했는지 (아주 중요!!!) 
		- .... 
	ㄹ. 힘들었던 점 
		- ~~~ 구현에 ~~~ 지식이 필요했는데 그 부분이 어려웠다. 
		  그래서 이 부분을 ~~~~ 하여 해결했다.
		- ~~~~ 하려고 했는데 ~~~ 한 이유로 쉽게 구현할 수 없었다.
		  그래서 이 부분을 ~~~~ 하여 해결했다.

	ㄷ. 부족한 점 (개선해야 할 점) 
		- ~~ 기능을 추가하려했으나 ~~~이유로 구현하지 못했다. 
		  ~~~ 게 구현하면 될 것 (어떻게 개선할 것인가)
		- 나는 ~~~~ 게 구현했는데, ~~~~ 방법도 좋을 것 같다.
	ㅁ. 더 넣고 싶은 멘트가 있으면 추가
	ㅂ. 동영상 링크(유튜브 외부 링크) 및 섬네일 
<div>
	<a href="https://www.youtube.com/watch?v=비디오id" target="_blank"><image src = "https://img.youtube.com/vi/비디오id/mqdefault.jpg"></a>	

</div>
	ㅅ. Javadoc 링크(repo 내부 링크)
