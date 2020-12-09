## SQL Quiz

- **등록일자가 2020/01/01 이전인 포켓몬들의 번호, 이름, 등록일자 알려줘**
```sql
SELECT no,name,regdate FROM pokemon WHERE DATE(regdate) < DATE('2020/01/01');
```

- **레벨이 5 이상 10 이하인 포켓몬들의 모든 정보 알려줘**
```sql
SELECT * FROM pokemon WHERE level>=5 AND level<=10;
```
- **모든 포켓몬들의 레벨 보여줘**
```sql
SELECT level FROM pokemon;
```
- **모든 포켓몬들의 레벨 보여줘. 중복 제거 해줘. (DISTINCT)**
```sql
SELECT DISTINCT level FROM pokemon;
```
- **모든 포켓몬들의 이름, 레벨 보여줘. 이름 오름차순으로 보여줘  (ORDER BY)**
```sql
SELECT name, level FROM pokemon ORDER BY name;
```
- **모든 포켓몬들의 이름, 레벨 보여줘. 레벨 많은 순으로 보여줘  (ORDER BY)**
```sql 
SELECT name, level FROM pokemon ORDER BY level ASC;
```
- **레벨이 3이상인 모든 포켓몬들의 이름, 레벨 보여줘. 레벨 많은 순으로,같은 레벨이면 이름 오름차순으로 보여줘(ORDER BY)**
```sql
SELECT name, level FROM pokemon WHERE level >=3 ORDER BY level ASC,name ASC;
```
- **모든 포켓몬들의 이름, 레벨 보여줘. 등록일자 최신순으로 보여줘.**
```sql
SELECT name, level FROM pokemon ORDER BY regdate DESC;
```
- **레벨이 100 미만인 포켓몬들의 레벨을 1씩 증가해줘**
```sql
UPDATE pokemon SET level = level+1 WHERE level <100;
```
- **포켓몬 중 레벨이 100 인 포켓몬들을 삭제해줘**
```sql
DELETE FROM pokemon WHERE level =100;
```
- **레벨이 짝수인 포켓몬들의 레벨을 두배로 변경해줘**
```sql
UPDATE pokemon SET level = level*2 WHERE level%2 = 0;
```
- **체력이 레벨의 100배보다 큰 포켓몬들의 레벨을 체력의 1/100로 수정해줘**
```sql
UPDATE pokemon SET hp = hp/100 WHERE hp > level*100;
```



## SQL Homework
### **1. MEMBER 테이블 만들기**
* **항목**
  * 회원번호(no), 아이디(id), 패스워드(password), 이메일(email), 등급(type), 적립금(point)
* **제약조건**
  Primary key 는 회원번호다.
  * 아이디는 중복이면 안된다. (UNIQUE) 
  * 이메일도 중복이면 안된다. (UNIQUE) 
  * 아이디는 누락되면 안된다. (NOT NULL)
  * 등급은 1 ~ 4가 있고 기본값은 1이다.
  * +적립금은 1000원이 기본값이고 음수일 수 없다.
 
 __*ANSWER*__
 ```sql
 CREATE TABLE MEMBER (
	no INT PRIMARY KEY AUTO_INCREMENT,
	id VARCHAR(40) NOT NULL UNIQUE,
            password VARCHAR(40),
            email VARCHAR(40) UNIQUE,
            type INT(3) DEFAULT '1' CHECK (type>=1 AND type <=4) ,
            point INT(5) DEFAULT '1000' CHECK (point>=0) );
```
__*RESULT*__

![hw1](https://user-images.githubusercontent.com/43839669/101602120-f0f77800-3a40-11eb-8d3c-131501b913a7.JPG)




### **2. QNA 테이블 만들기**

* **항목** 
  * 질문번호(no), 글쓴이번호(writer_no), 질문 내용(content), 등록일자(regdate)
* **제약조건**
  * Primary key 는 질문번호다.
  * 글쓴이번호는 MEMBER 테이블의 회원번호를 참조한다. (FOREIGN KEY)
  * 글쓴이 회원이 삭제되면 해당 질문도 삭제된다. (CASCADE)	
  * 질문 내용은 누락되면 안된다.
  * 등록일자는 기본값이 현재 시간이다. 
   
 
__*ANSWER*__
 
```sql
CREATE TABLE QNA (
	no INT PRIMARY KEY AUTO_INCREMENT,
            writer_no INT,
            content VARCHAR(100) NOT NULL,
            regdate DATETIME DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY(writer_no) REFERENCES MEMBER(no) ON DELETE CASCADE);
```
__*RESULT*__

![hw2](https://user-images.githubusercontent.com/43839669/101602218-1be1cc00-3a41-11eb-8af6-7ed896c212fd.JPG)
