# 판매자 메인페이지 승인대기중, 판매중, 거부된항목 처리

![image](https://user-images.githubusercontent.com/106857769/187033146-8c6b445d-a983-450a-813d-c7aa4a6ee628.png)

~~~
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts/sellertag" %>
<%
	request.setAttribute("addCss", new String[]{"/seller/index"});
	request.setAttribute("addJs", new String[]{"/seller/index"});


%>
<layout:seller>
	<div class="background"></div>
		<div class="outerbox">
			<div class="box userinfo">
				<span class="xi-profile-o"></span>	
					
			</div>
			<div class="box">
				<span>이름 : ${member.name }</span>			
				<span>이메일 : ${member.email }</span>				
				<span>전화번호 : ${member.mobile }</span>				
			</div>
		</div>
		
		<div class="box productInfo">
			<div class="reqCount">
				<span>승인 대기중</span>
				<span>${reqCount }개</span>
			
			</div>
			<div class="sellCount">
				<span>판매 중</span>
				<span>${trueCount }개</span>
			 </div>
			 
			 <div class="sellCount">
				<span> 거부된 항목</span>
				<span>${falseCount }건</span>
			 </div>
		</div>
	

</layout:seller>
~~~
# IndexController
~~~
package controllers.seller;

import java.io.IOException;
import static jmsUtil.Utils.*;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import models.seller.IndexService;
import models.seller.LoginException;

@WebServlet("/seller")
public class IndexController extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		try {
			IndexService service=new IndexService();
			service.process(req);
			
			RequestDispatcher rd=req.getRequestDispatcher("/jmsPage/index.jsp");
			rd.forward(req, resp);
		}catch (LoginException e) {
			RequestDispatcher rd=req.getRequestDispatcher("/seller/login");
			rd.forward(req, resp);

		}
		
		catch (Exception e) {
			showAlertException(resp, e);
		}
		
	}

}

~~~
# IndexService 
- 로그인 예외처리
~~~
  UserDto loginUser = getLoginUser(req);

		if (loginUser==null) {
			throw new  LoginException();
		}
~~~
 - getLoginUser(HttpServletRequest req)

  ~~~
	public static UserDto getLoginUser(HttpServletRequest req) {
		HttpSession session=req.getSession();
		UserDto user=(UserDto) session.getAttribute("member");
		
		return user;
	}
  ~~~

- DB 에서 각각의 값 가져오기
~~~
SellerDao dao = SellerDao.getInstance();

		ProductDto product = new ProductDto();
		product.setSeller(loginUser.getId());

		product.setStatus("req");
		int reqCount = dao.getlistCount(product);

		product.setStatus("true");
		int trueCount = dao.getlistCount(product);

~~~
- SellerDao 세팅
~~~ 
	public int getlistCount(ProductDto product) {
		SqlSession session=Connection.getSession();
		int count =session.selectOne("RequestProductMap.getlistCount",product);
		
		session.close();
		
		return count;
	}
~~~
- requestProductMapper
~~~
<!-- seller : 신청자(Userid) , status : 상태값(요청중(req),승인(ture),거부(false)) 조건으로 상품 갯수 가져옴 -->
<select id="getlistCount" resultType="int">
	SELECT COUNT(*) FROM RequestProduct WHERE seller=#{seller} AND status=#{status};
</select>
~~~

- DB 에서 받은 값 설정해주기
~~~
		
		req.setAttribute("reqCount", reqCount);
		req.setAttribute("trueCount", trueCount);
		req.setAttribute("falseCount", falseCount);
~~~

# IndexService 전반
~~~
package models.seller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import com.mysql.cj.Session;

import dto.UserDto;

import static jmsUtil.Utils.*;

public class IndexService {
	public void process(HttpServletRequest req) {
		UserDto loginUser = getLoginUser(req);

		if (loginUser==null) {
			throw new  LoginException();
		}
		
		SellerDao dao = SellerDao.getInstance();

		ProductDto product = new ProductDto();
		product.setSeller(loginUser.getId());

		product.setStatus("req");
		int reqCount = dao.getlistCount(product);

		product.setStatus("true");
		int trueCount = dao.getlistCount(product);

		product.setStatus("false");
		int falseCount = dao.getlistCount(product);
		
		req.setAttribute("reqCount", reqCount);
		req.setAttribute("trueCount", trueCount);
		req.setAttribute("falseCount", falseCount);

	}
}

~~~
