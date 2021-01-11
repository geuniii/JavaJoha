## 문제2
<br>

![quiz01](https://user-images.githubusercontent.com/62981623/104227216-687b5580-548c-11eb-93d9-df8ff9e850a2.JPG)

```
1. 혈액형 선택받기(Radio 사용)
	O / A / AB / B / 기타 
2. 통신사 선택받기(Radio 사용)
	LG UPLUS / KT / SK / 기타
3. 희망 과목 선택받기 (checkbox 사용) - 중복 선택 가능
    C++ / Python / Java / 기타
4. submit (action="Quiz01" method="post")
	<input type="submit" name="result" value="아주 좋음">
    <input type="submit" name="result" value="좋음">
    <input type="submit" name="result" value="보통">
    <input type="submit" name="result" value="별로">
    <input type="submit" name="result" value="아주 별로">
    
[Quiz01 서블릿]
받은 파라미터를 sysout으로 출력
result 의 값을 화면으로 출력
```

### Code

- quiz01_form.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h2>설문조사 페이지 만들기</h2>
<form action="/ch02_homework/Quiz01" method="post">
   <br>혈액형을 선택하세요!<br>
  <input type="radio" id="o" name="blood" value="O">
  <label for="o">O형</label><br>
    <input type="radio" id="a" name="blood" value="A">
  <label for="a">A형</label><br>
    <input type="radio" id="b" name="blood" value="B">
  <label for="b">B형</label><br>
    <input type="radio" id="ab" name="blood" value="AB">
  <label for="ab">AB형</label><br>
  <input type="radio" id="other" name="blood" value="other">
  <label for="other">기타</label><br>


   <br>통신사를 선택하세요!<br>
  <input type="radio" id="lg" name="agency" value="LG Uplus">
  <label for="lg">LG U Plus</label><br>
  <input type="radio" id="kt" name="agency" value="KT">
  <label for="kt">KT</label><br>
    <input type="radio" id="sk" name="agency" value="SK">
  <label for="sk">SKT</label><br>
  <input type="radio" id="other" name="agency" value="other">
  <label for="other">기타</label><br>



  <br>희망 과목을 선택하세요!<br>
  <input type="checkbox" id="cpp" name="subject" value="C++">
  <label for="cpp">C++</label><br>
  <input type="checkbox" id="python" name="subject" value="Python">
  <label for="python">Python</label><br>
  <input type="checkbox" id="java" name="subject" value="JAVA">
  <label for="java">JAVA</label><br>
  <input type="checkbox" id="other" name="subject" value="other">
  <label for="other">기타</label><br>

  <br>
 
    <input type="submit" name="result" value="아주 좋음">
    <input type="submit" name="result" value="좋음">
    <input type="submit" name="result" value="보통">
    <input type="submit" name="result" value="별로">
    <input type="submit" name="result" value="아주 별로">

 </form> 
</body>
</html>
```
<br><br>

- Quiz01.java

```java
package homework;

import java.io.IOException;
import java.util.Arrays;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Quiz01")
public class Quiz01 extends HttpServlet {

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

		req.setCharacterEncoding("UTF-8");
		resp.setContentType("text/html;charset=UTF-8");
		String blood = req.getParameter("blood");
		String agency = req.getParameter("agency");
		String[] subArr = req.getParameterValues("subject");
		String result = req.getParameter("result");
		
		System.out.println("혈액형: "+blood);
		System.out.println("통신사: "+agency);
		System.out.println("좋아하는 과목: "+Arrays.toString(subArr));
		
		String html ="<script>alert('"+result+"');</script>";	
		
		resp.getWriter().write(html);
		
	}

	
}
```

