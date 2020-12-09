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
