## 문제1_ 회원가입 기능

<br>

![test01](https://user-images.githubusercontent.com/62981623/104226504-4208ea80-548b-11eb-986a-6ae9b9334369.JPG)



```
[test03-form.html]
회원가입 <form>을 하나 만들고
action : /ch02_servlet/Test03 
method : post
		 

아이디- <input> text 
비밀번호 2번- <input> password
이메일- <input> text + <select> naver.com / nate.com / gmail.com / hanmail.net 
운영진에게 한마디- <textarea>
등 등...

[Test03 서블릿] 
인자로 들어온 파라미터를 모두 받아 sysout 으로 출력
두 비밀번호가 같으면 "회원 가입 완료!"
다르면 "비밀번호가 일치하지 않습니다." 를 화면에 출력
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

	<!--회원가입 <form>을 하나 만들고
      action : /ch02_servlet/Test03
      method : post
      아이디-<input>text
      비밀번호 2번-<input>password
      이메일-<input>text + <select> naver.com / nate.com/ hanmail.net
      운영진에게 한마디-<textarea>
      등등...
      
      Test03 서블릿
      인자로 들어온 파라미터를 모두 받아 sysout으로 출력
      두 비밀번호가 같으면 "회원가입 완료!"
      다르면 "비밀번호가 일치하지 않습니다!"
      
      -->
	<div class="wrap">
		<form action="/ch02_homework/Test03" method="post">
			<fieldset class="text-center scheduler-border" style="width: 400px; height: auto">
				<legend class="scheduler-border">JOIN US!</legend>
				<br> <img
					src="https://user-images.githubusercontent.com/62981623/103341879-63e99100-4acb-11eb-899e-82f7d14d5e56.jpg"
					class="rounded-circle" alt="Cinque Terre"
					style="width: 100px; height: auto;">

				<div class="form-group">
					<br> <label for="f_id">ID</label> <input type="text" name="id"
						id="f_id" placeholder="Write your ID" required="required"><br>
				</div>
				<div class="form-group">
					<label for="f_pw">PW</label> <input type="password" name="pw1"
						id="f_pw" placeholder="Write your PW" required="required"><br>
				</div>
				<div class="form-group">
					<label for="f_pw">PW</label> <input type="password" name="pw2"
						id="f_pw2" placeholder="One more PW" required="required"><br>
				</div>
				<div class="form-group">
					<label for="email">Email</label>
					<input type="text" name="email_id" id="email_id"
						placeholder="Write your Email" required="required"> <select
						name='email'>
						<p>@</p>
						<option value='' selected>-- Select Email--</option>
						<option value='naver.com'>naver.com</option>
						<option value='nate.com'>nate.com</option>
						<option value='hanmail.net'>hanmail.net</option>
					</select> <br>
				</div>
				<div class="form-group">
				<label for="f_msg">Message</label><br>
                <textarea id="f_msg" name="message"rows="4" cols="50"></textarea>
				</div>
				<br><br>
				<input type="submit" value="회원가입!" />
			</fieldset>
		</form>
	</div>


</body>
</html>
```

<br><br>
- Test03.java
```java
package homework;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Test03")
public class Test03 extends HttpServlet{
	

		@Override
		protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
			
			req.setCharacterEncoding("UTF-8");
			resp.setContentType("text/html;charset=UTF-8");
			
			String id = req.getParameter("id");
			String pw1 = req.getParameter("pw1");
			String pw2 = req.getParameter("pw2");
			String email_id = req.getParameter("email_id");
			String email = req.getParameter("email");
			String message = req.getParameter("message");

			System.out.println("ID: " + id);
			System.out.println("PW1: " + pw1);
			System.out.println("PW2: " + pw2);
			System.out.println("Email: " + email_id+"@"+email);
			System.out.println("Message: " + message);
			
			boolean result = false;
			if(pw1.equals(pw2)) {
				result = true;

			}
			String html = "<script>alert(" + 
					(result?"'회원가입 성공!'":"'회원가입 실패!'") +
					");</script>";	
			
			resp.getWriter().write(html);
		}
	
}
```
