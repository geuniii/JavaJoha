## 문제3_구구단 문제

![quiz02](https://user-images.githubusercontent.com/62981623/104227975-885f4900-548d-11eb-89e9-9355d908098b.JPG)


```
랜덤하게 구구단 1문제를 내고 값을 입력 받는 <form> 구현
"정답!" 혹은 "땡!"을 alert() 하기
(서블릿, jsp, html 아무거나 선택)
```

### Code
- quiz02.jsp

```html
<%@page import="java.util.HashMap"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>


<h2>구구단 게임!</h2>

<%
int randDan =(int)((Math.random()*9)+2);
int randN =(int)((Math.random()*9)+1);
String quiz = Integer.toString(randDan)+"X"+Integer.toString(randN)+"= ?";
System.out.println(quiz);
%>

<br>
<form>

<div>
  <span id = "randDan"><%=randDan%></span> 
  X
  <span id = "randN"><%=randN%></span>
  = ?
</div>

<br>
  <input type="text" id="answer" placeholder="Write Answer" required="required">
  <input type="button" value="확인!" onclick ='confirm()'>
</form>


<script>
function confirm(){
	var randDan =parseInt(document.getElementById("randDan").innerHTML);
	var randN = parseInt(document.getElementById("randN").innerHTML);
	var answer =parseInt(document.getElementById("answer").value);

	   if(randDan*randN==answer){
		   alert("정답입니다!");
	   }
	   else{
		   alert("땡!!!")
	   }
}
</script>

</body>
</html>
```
