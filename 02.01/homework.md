## 대댓글 달기 / 댓글 삭제 

![댓글보기](https://user-images.githubusercontent.com/62981623/106800284-99bdfe80-66a3-11eb-95ca-d40ee69b94fd.jpg)



![댓글달기](https://user-images.githubusercontent.com/62981623/106800274-97f43b00-66a3-11eb-90fb-db06f085c25f.JPG)





#### - DB

```mysql
CREATE TABLE comments (
	id BIGINT PRIMARY KEY AUTO_INCREMENT,
            brd_id BIGINT NOT NULL,
            parent_id BIGINT DEFAULT '0',
            content VARCHAR(255) NOT NULL,
            writer BIGINT NOT NULL,
	regdate DATETIME DEFAULT CURRENT_TIMESTAMP
);


ALTER TABLE  comments
ADD CONSTRAINT FK_parent_id 
FOREIGN KEY (parent_id)  REFERENCES comments(id) ON DELETE CASCADE;

ALTER TABLE  comments
ADD CONSTRAINT FK_brd_id 
FOREIGN KEY (brd_id)  REFERENCES board(id) ON DELETE CASCADE;

ALTER TABLE  comments
ADD CONSTRAINT FK_writer
FOREIGN KEY (writer)  REFERENCES user(id) ON DELETE CASCADE;
```



#### - code

- boardDetail.jsp

  ```jsp
  
  <!-- 댓글 입력받기 -->
  <c:if test="${currentUser != null }">
      <div class="row">
          <p class="col-1" style="text-align:right">${currentUser.nickname}: </p>
          <input name="context" style="text-align:center" type="text" class="col-10 form-control" id="content"
                 placeholder="Write down your comments">
          <button class="lg_btn btn-dark" onclick="commentWrite();">SUBMIT</button>
      </div>
  </c:if>
  
  <br/>
  
  <!-- 댓글 리스트 -->
  <c:forEach var="dto" items="${list }">
  
      <!-- 대댓글이 아니면 -->
      <c:if test="${ dto.parentId ==0 }">
          <div class="row" style="text-align:center">
  
              <p class="col-1">${ dto.nickname }</p>
              <p class="col-7">${ dto.content }</p>
              <p class="col-3">${ dto.regdate }</p>
              <c:if test="${currentUser.id == dto.writer}">
                  <button class="btn btn-link" style="color:gray;font-size:small"
                          onclick="deleteComment(${ dto.id });">DELETE
                  </button>
              </c:if>
              <!-- 대댓글이 있으면 -->
              <c:if test="${map.get(dto.id)!=null}">
                  <details class="col-12">
                      <summary style="font-size:xx-small; text-align:left">답글</summary>
                      <br/>
  
                      <div class="row" style="text-align:center">
                          <p class="font-italic" style="text-align:right">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└──────></p>
                          <p class="font-italic">&nbsp;&nbsp;&nbsp;${ map.get(dto.id).nickname } )</p>
                          <p class="font-italic">&nbsp;&nbsp;&nbsp;${ map.get(dto.id).content }</p>
                          <p class="font-italic">&nbsp;&nbsp;&nbsp;${ map.get(dto.id).regdate }</p>
                          <c:if test="${currentUser.id ==  map.get(dto.id).writer}">
                              <button class="btn btn-link" style="color:gray;font-size:small"
                                      onclick="childCommentDelete(${ map.get(dto.id).id });">DELETE
                              </button>
                          </c:if>
  
                      </div>
  
                  </details>
              </c:if>
  
  
              <!-- 대댓글이 없으면 -->
              <c:if test="${ map.get(dto.id)==null}">
                  <details class="col-12">
                      <summary style="font-size:xx-small; text-align:left">답글</summary>
                      <br/>
  
                      <div class="row">
                          <input name="context" type="text" class="col-11 form-control" id="childContent"
                                 placeholder="Write down your comments">
                          <button class="col-1 btn btn-link btn-sm " style="color:gray"
                                  onclick="childCommentWrite(${ dto.id })">SUBMIT
                          </button>
                      </div>
  
                  </details>
              </c:if>
          </div>
          <hr style="height:1px;border-width:0;color:gray;background-color:gray">
      </c:if>
  </c:forEach>
  </div>
  
  <script>
  
      function deleteBoardAjax() {
          $.ajax({
              type: 'get',
              url: '${pageContext.request.contextPath}/board/delete',
              data: "id=${dto.id}",
              dataType: 'json',
              success: () => {
                  console.log("deleteBoardAjax()성공");
              },
              error: () => {
                  console.log("deleteBoardAjax()실패");
              }
          });
      }
  
      function showModal(modalTitle, modalContent) {
          $('.modal-title').html(modalTitle);
          $('.modal-body').html(modalContent);
          $('#exampleModalCenter').modal('show'); // modal 창 띄우기
      }
  
      function commentWrite() {
  
          console.log($("#content").val());
          $.ajax({
              type: 'post',
              url: '${pageContext.request.contextPath}/board/commentwrite',
  
              data: {boardId: ${dto.id}, content: $("#content").val()},
              dataType: 'json',
              success: () => {
                  window.location.href = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
              },
              error: () => {
                  console.log("commentWrite()실패");
  
              }
          });
      }
  
      function deleteComment(commentId) {
          console.log(commentId);
          $.ajax({
              type: 'post',
              url: '${pageContext.request.contextPath}/board/commentDelete',
              data: {"id": commentId},
              dataType: 'json',
              success: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
              },
              error: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
              }
          });
      }
  
      function childCommentWrite(parentId) {
          $.ajax({
              type: 'post',
              url: '${pageContext.request.contextPath}/board/commentwrite',
  
              data: {"boardId": ${dto.id}, "parentId": parentId, "content": $("#childContent").val()},
              dataType: 'json',
              success: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
              },
              error: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
  
              }
          });
      }
  
      function childCommentDelete(commentId) {
          $.ajax({
              type: 'post',
              url: '${pageContext.request.contextPath}/board/commentDelete',
  
              data: {"id": commentId},
              dataType: 'json',
              success: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
              },
              error: () => {
                  window.location = '${pageContext.request.contextPath }/board/detail?id=${dto.id }';
  
  
              }
          });
      }
  
  
  </script>
  
  
  </jsp:body>
  </t:commonLayout>
  
  
  
  
  
  
  
  ```





- Mycontroller.java

  ```java
  case "/commentlist": {
  			String bId = request.getParameter("boardId");
  			List<CommentDto> list = commentService.getCommentList(Long.parseLong(bId));
  
  			request.setAttribute("list", list);
  			request.getRequestDispatcher("/brd/boardDetail.jsp").forward(request, response);
  		}
  			break;
  
  		case "/childComment": {
  			String bId = request.getParameter("boardId");
  			String pId = request.getParameter("parentId");
  
  			CommentDto childDto = commentService.getChildComment(Long.parseLong(bId), Long.parseLong(pId));
  			response.getWriter().write("'childDto': childDto");
  		}
  			break;
  
  		case "/commentwrite": {
  
  			if (currentUser == null) {
  				request.setAttribute("error_message", "invalid.session");
  				request.getRequestDispatcher("/board/detail").forward(request, response);
  				return;
  			}
  
  			long boardId = Long.parseLong(request.getParameter("boardId"));
  			String content = request.getParameter("content");
  			long writer = currentUser.getId();
  
  			if (request.getParameter("parentId") != null) {
  				long parentId = Long.parseLong(request.getParameter("parentId"));
  				commentService.save(boardId, parentId, content, writer);
  				request.getRequestDispatcher("/board/detail?id=" + boardId).forward(request, response);
  
  			} else {
  				commentService.save(boardId, content, writer);
  				request.getRequestDispatcher("/board/detail?id=" + boardId).forward(request, response);
  
  			}
  		}
  			break;
  
  		case "/commentDelete": {
  			String bId = request.getParameter("id");
  			JsonObject json = new JsonObject();
  			if (bId == null || bId.isEmpty()) {
  				json.addProperty("result", false);
  				response.getWriter().write(json.toString());
  				return;
  			}
  			commentService.deleteComment(Long.parseLong(bId));
  			request.getRequestDispatcher("/board/detail?id=" + bId).forward(request, response);
  		}
  			break;
  
  ```

  

- CommentService.java

  ```java
  package com.megait.service;
  
  import java.util.List;
  
  import com.megait.domain.CommentDto;
  import com.megait.repository.CommentDao;
  
  public class CommentService {
  
  	private static CommentService instance;
  
  	public static CommentService getInstance() {
  		if (instance == null) {
  			instance = new CommentService();
  		}
  		return instance;
  	}
  
  	private CommentDao commentDao = CommentDao.getInstance();
  
  	public List<CommentDto> getCommentList(long boardId) {
  		return commentDao.findAll(boardId);
  	}
  
  	public boolean save(long boardId, long parentId, String content, long writer) {
  
  		CommentDto dto = new CommentDto();
  		dto.setBoardId(boardId);
  		dto.setParentId(parentId);
  		dto.setContent(content.replace("\n", "<br/>"));
  		dto.setWriter(writer);
  
  		commentDao.save(dto);
  		return true;
  	}
  
  	public boolean save(long boardId, String content, long writer) {
  
  		CommentDto dto = new CommentDto();
  		dto.setBoardId(boardId);
  		dto.setContent(content.replace("\n", "<br/>"));
  		dto.setWriter(writer);
  
  		commentDao.save(dto);
  		return true;
  	}
  
  	public void deleteComment(long id) {
  		commentDao.delete(id);
  	}
  
  	public CommentDto getChildComment(long boardId, long parentId) {
  		CommentDto dto = commentDao.getChildComment(boardId, parentId);
  		return dto;
  
  	}
  }
  
  ```

  

- CommentDao.java

  ```java
  package com.megait.repository;
  
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.util.ArrayList;
  import java.util.List;
  
  import com.megait.domain.BoardDto;
  import com.megait.domain.CommentDto;
  
  public class CommentDao {
  	private static CommentDao instance;
  
  	private CommentDao() {
  		try {
  			Class.forName("com.mysql.jdbc.Driver");
  		} catch (ClassNotFoundException e) {
  			e.printStackTrace();
  		}
  	}
  
  	public static CommentDao getInstance() {
  		if (instance == null) {
  			instance = new CommentDao();
  		}
  		return instance;
  	}
  	////////////////////////////////////////////////
  
  	private Connection conn;
  	private PreparedStatement ps;
  	private ResultSet rs;
  
  	public void save(CommentDto dto) {
  		String sql = "INSERT INTO comments (brd_id,parent_id,content,writer) " + "VALUES (?, ?, ?, ?)";
  		try {
  			conn = ConnectionUtil.getConnection();
  			ps = conn.prepareStatement(sql);
  			ps.setLong(1, dto.getBoardId());
  			ps.setLong(2, dto.getParentId());
  			ps.setString(3, dto.getContent());
  			ps.setLong(4, dto.getWriter());
  
  			if (ps.executeUpdate() < 1) { // insert 실패
  				dto = null;
  			}
  		} catch (Exception e) {
  			e.printStackTrace();
  		} finally {
  			ConnectionUtil.close(conn, ps);
  		}
  	}
  
  	public List<CommentDto> findAll(long boardId) {
  		String sql = "SELECT c.*,u.nickname \r\n" + "FROM comments c\r\n" + "JOIN user u\r\n" + "ON c.writer = u.id\r\n"
  				+ "WHERE brd_id=?";
  		List<CommentDto> list = new ArrayList<CommentDto>();
  
  		CommentDto dto = null;
  
  		try {
  			conn = ConnectionUtil.getConnection();
  			ps = conn.prepareStatement(sql);
  
  			ps.setLong(1, boardId);
  
  			rs = ps.executeQuery();
  
  			while (rs.next()) {
  
  				dto = new CommentDto();
  				dto.setId(rs.getLong("c.id"));
  				dto.setBoardId(rs.getLong("c.brd_id"));
  				dto.setParentId(rs.getLong("c.parent_id"));
  				dto.setContent(rs.getString("c.content"));
  				dto.setWriter(rs.getLong("c.writer"));
  				dto.setRegdate(rs.getString("c.regdate"));
  				dto.setNickname(rs.getString("u.nickname"));
  				list.add(dto);
  			}
  
  		} catch (Exception e) {
  			e.printStackTrace();
  		} finally {
  			ConnectionUtil.close(conn, ps, rs);
  		}
  		return list;
  	}
  
  	public void delete(long id) {
  
  		String sql = "DELETE FROM comments WHERE id = ?";
  		try {
  			conn = ConnectionUtil.getConnection();
  			ps = conn.prepareStatement(sql);
  			ps.setLong(1, id);
  			System.out.println("여기 delete();" + ps.executeUpdate());
  		} catch (Exception e) {
  			e.printStackTrace();
  		} finally {
  			ConnectionUtil.close(conn, ps);
  		}
  	}
  
  	public CommentDto getChildComment(long boardId, long parentId) {
  		String sql = "SELECT c.*,u.nickname \r\n" + "FROM comments c\r\n" + "JOIN user u\r\n" + "ON c.writer = u.id\r\n"
  				+ "WHERE brd_id=? AND parent_id=?";
  
  		CommentDto dto = null;
  
  		try {
  			conn = ConnectionUtil.getConnection();
  			ps = conn.prepareStatement(sql);
  			ps.setLong(1, boardId);
  			ps.setLong(2, parentId);
  			rs = ps.executeQuery();
  
  			if (rs.next()) {
  				dto = new CommentDto();
  				dto.setId(rs.getLong("c.id"));
  				dto.setBoardId(rs.getLong("c.brd_id"));
  				dto.setParentId(rs.getLong("c.parent_id"));
  				dto.setContent(rs.getString("c.content"));
  				dto.setWriter(rs.getLong("c.writer"));
  				dto.setRegdate(rs.getString("c.regdate"));
  				dto.setNickname(rs.getString("u.nickname"));
  			}
  
  		} catch (Exception e) {
  			e.printStackTrace();
  		} finally {
  			ConnectionUtil.close(conn, ps, rs);
  		}
  		return dto;
  	}
  
  	public void update(long id, CommentDto dto) {
  		String sql = "UPDATE comments SET content=? WHERE id = ?";
  		try {
  			conn = ConnectionUtil.getConnection();
  			ps = conn.prepareStatement(sql);
  			ps.setString(1, dto.getContent());
  			ps.setLong(2, id);
  			ps.executeUpdate();
  		} catch (Exception e) {
  			e.printStackTrace();
  		} finally {
  			ConnectionUtil.close(conn, ps);
  		}
  	}
  
  }
  
  ```

  

