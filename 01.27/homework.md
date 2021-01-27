### 게시판 글 삭제 / 수정

#### 1. 삭제 

![delete](https://user-images.githubusercontent.com/62981623/106024798-7a572c80-610b-11eb-8c3d-371b83828a57.jpg)

- MyController.java

  ```java
  case "/delete": {
  
  			String id = request.getParameter("id");
  			int writer = Integer.parseInt(request.getParameter("writer"));
  			System.out.println("writer:" + writer + "currentUser.getId():" + currentUser.getId());
  
  			// 로그인해서 접근하지 않거나 작성자와 다를 경우
  			if (currentUser == null || currentUser.getId() != writer) {
  				request.setAttribute("delete_error_message", "invalid.session");
  				request.getRequestDispatcher("/board/list").forward(request, response);
  				return;
  			}
  
  			boardService.delete(Long.parseLong(id));
  
  			request.setAttribute("delete_success_message", "delete.complete");
  			request.getRequestDispatcher("/board/list").forward(request, response);
  
  		}
  			break;
  ```



- boardList.jsp -->modal추가

  ```jsp
  
  		<c:when test="${delete_error_message == 'invalid.session'}">
  			<script>
  			$(document).ready(()=>{
  				$('.modal-title').html("요청 실패");
  				$('.modal-body').html("작성자만 글을 삭제하실 수 있습니다.");
  				$('#exampleModalCenter').modal('show'); // modal 창 띄우기
  			});
  			</script>
  		</c:when>
  		
  	    <c:when test="${delete_success_message == 'delete.complete'}">
  			<script>
  			$(document).ready(()=>{
  				$('.modal-title').html("글삭제");
  				$('.modal-body').html("글을 성공적으로 삭제하였습니다.");
  				$('#exampleModalCenter').modal('show'); // modal 창 띄우기
  			});
  			</script>
  		</c:when>
  ```





#### 2. 수정

![revise](https://user-images.githubusercontent.com/62981623/106023270-fe101980-6109-11eb-977e-1195beffdc5c.jpg)



- boardUpdate.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@taglib prefix="t" tagdir="/WEB-INF/tags"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<t:commonLayout title="board" nav="board">
	<jsp:body>
 
	<div class="container" style="color: white"> 
            <div class="row">
             
                <table class="table" style="color: white">
               
                    <thead> 
                    <tr class="table-active">
                        <th>
                        <!-- 아이디 검사  -->
                        <input type="hidden" id="id" name="id"
								value="${dto.id}" />
                        
                        <label for="title">제목:</label>
                        <input type="text" class="form-control"
								id="title" placeholder="Write title" name="title"
								value="${dto.title }" required>
						</th>						
                        <th scope="col" style="width: 15%"
								class="text-right"> 작성자: ${dto.nickname}</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr>
                    <td>
                       <textarea
									placeholder="This is textarea. Input some text..."
									style="height: 400px; width: 100%" id="content" name="content">${dto.content }</textarea>
                    </td>
                    </tr>
                    <tr>
                 <td> 
                  <div class="text-center">
                            <button type="submit"
										class="lg_btn btn-dark" onclick="submit();">REVISE</button>
                  </div>
        </td>
        </tr>
                    </tbody>
                    
                </table>
            </div>
       </div> 
     
       
       <script>
		function submit() {
			var title = $("#title").val();
			var content = $("#content").val();
			var id = $("#id").val();
			console.log($("#id").val())
			$
					.ajax({
						type : "post",//요청방식
						url : '/myhome/board/update',//요청할 url (파라미터를 보낼 곳)
						data : {
							"title" : title,
							"content" : content,
							"id" : id
						},
						error : function(xhr, status,
								error) {//통신 실패(2XX 응답이 아닌 다른 응답코드)
							alert(error);
						},
						success : function(data) {
							window.location.href = "${pageContext.request.contextPath }/board/detail?id=${dto.id}"
						}

					});
		}
					
	  </script>
       
	
</jsp:body>
</t:commonLayout>
```





- MyController.java

```java
case "/update": {

			// GET방식
			if ("GET".equals(request.getMethod())) {
				int writer = Integer.parseInt(request.getParameter("writer"));
				System.out.println("writer:" + writer + "currentUser.getId():" + currentUser.getId());
				String bId = request.getParameter("id");
				// 글 id 안넘어오면
				if (bId == null || bId.isEmpty()) {
					request.setAttribute("error_message", "invalid.parameter");
					request.getRequestDispatcher("/board/detail").forward(request, response);
					return;
				}

				// 로그인 안했거나, 작성자가 아니면
				if (currentUser == null || currentUser.getId() != writer) {
					request.setAttribute("update_error_message", "invalid.session");
					request.getRequestDispatcher("/board/list").forward(request, response);
					return;
				}

				long id = Long.parseLong(bId);
				BoardDto dto = boardService.getBoard(id, false);
				request.setAttribute("dto", dto);
				request.getRequestDispatcher("/brd/boardUpdate.jsp").forward(request, response);
			}

			// POST방식
			else {
				BoardDto dto = new BoardDto();

				long id = Long.parseLong(request.getParameter("id"));
				dto.setTitle(request.getParameter("title"));
				dto.setContent(request.getParameter("content"));

				boardService.update(id, dto);
			}
		}
			break;
		}
```

