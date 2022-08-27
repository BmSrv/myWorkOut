# 승인요청한 상품 목록

![image](https://user-images.githubusercontent.com/106857769/187017358-44d7a618-ae3f-43da-bfae-234058d2389f.png)
- myProduct.jsp
~~~
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts/sellertag" %>
<%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core" %>
<link rel="stylesheet" href="<c:url value="/static/css/seller/myProduct.css" />" />
	<%
		String thispage= request.getParameter("status");
	%>
	<c:set value="<%=thispage %>" var="status" />
<layout:seller>
	<div class="background"></div>
	<div class="box">

	<h1>
		<c:choose>
			<c:when test="${status == 'req' }">
				승인 대기중 목록
			</c:when>
			
			<c:when test="${status == 'true' }">
				승인된 목록
			</c:when>
			
			
			<c:when test="${status == 'false' }">
				거부된 목록
			</c:when>
		</c:choose>
	</h1>
	<!-- abnum, seller, serialnum, name, price, kategorie, company, img, status -->
	<table>
		<thead>
			<tr>
				<td>요청 번호</td>
				<td>serialnum</td>
				<td>name</td>
				<td>price</td>
				<td>kategorie</td>
				<td>company</td>
				<td></td>
			</tr>
		</thead>
		<tbody>
		<!-- '28', NULL, 'MK1H3KH/A', 'Apple 2021 맥북프로 16', '4710000', '/가전제품/노트북/MacOs', 'Apple', '스크린샷 2022-08-26 시간: 02.31.12.png', 'req'
 -->
 		<c:forEach var="product" items="${list }">
 			<tr>
 				
				<td>${product.abnum }</td>
				<td>${product.serialnum }</td>
				<td>${product.name }</td>
				<td>${product.price }</td>
				<td>${product.kategorie }</td>
				<td>${product.company }</td>
				<td><a href="<c:url value="/seller/ReqRemove?abnum=${product.abnum}" />" >삭제</a></td>
			</tr>
 		
 		</c:forEach>
	
		</tbody>
		
	</table>
	</div>
</layout:seller>
~~~

# MyProductController.java
- GET : 해당 주소에 접속하면 (?status=?) 파리미터값으로받은 값을 확인하여 각각의 상품 목록을 만들어서 myProduct.jsp 에 전달해준다

~~~
package controllers.seller;

import java.io.IOException;
import static jmsUtil.Utils.*;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import models.seller.GetProductListService;
import models.seller.LoginException;
import models.seller.ProductDto;

@WebServlet("/seller/myproduct")
public class MyProductController extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		try {
			GetProductListService service=new GetProductListService();
			List<ProductDto> list =service.getlist(req);
			req.setAttribute("list", list);
			RequestDispatcher rd =req.getRequestDispatcher("/jmsPage/myProduct.jsp");
			rd.forward(req, resp);
			
		}catch (LoginException e) {
			showAlertException(resp, e);
			replacePage(resp, req.getContextPath()+"/seller/login", "parent");
		} catch (RuntimeException e) {
			e.printStackTrace();
		}
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
	}
}

~~~

# GetProductListService 
- status 값이 존재하지 않을때 예외처리
~~~
  Map<String, String> check=new HashMap<>();
	check.put("status", "잘못된 요청 입니다.");
		
	ckNullParaMap(req,check);
~~~
~~~
public static void ckNullParaMap(HttpServletRequest req,Map<String, String> msg ) {

		for(Entry<String, String> en: msg.entrySet() ){
			String key=en.getKey();
			String value=en.getValue();
			
			if (req.getParameter(key)==null||req.getParameter(key).isBlank()) {
				throw new RuntimeException(value);
			}
			
		};
		
	}

~~~

- 미 로그인시 예외처리
~~~
HttpSession session=req.getSession();
		UserDto userDto=(UserDto)session.getAttribute("member");
		
		if (userDto==null) {
			throw new LoginException();
		}
~~~

- 로그인 정보 가져오기
~~~
HttpSession session=req.getSession();
UserDto userDto=(UserDto)session.getAttribute("member");
		
String seller=userDto.getId();
		
~~~

- status 값 가져오기
~~~
String status=req.getParameter("status");

~~~

- DB에 접속하여 List 가져오기
~~~
ProductDto product=new ProductDto();
product.setSeller(seller);
product.setStatus(status);
		
dao.getProductList(product);
~~~

- myProduct.jsp 에 list 넘겨주기(MyProductController)
~~~
List<ProductDto> list =service.getlist(req);
req.setAttribute("list", list);
RequestDispatcher rd =req.getRequestDispatcher("/jmsPage/myProduct.jsp");
rd.forward(req, resp);
~~~
# GetProductListService 전반
~~~
package models.seller;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import static jmsUtil.Utils.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import dto.UserDto;

public class GetProductListService {
	public List<ProductDto> getlist(HttpServletRequest req){
		showParaMap(req);
		
		Map<String, String> check=new HashMap<>();
		check.put("status", "잘못된 요청 입니다.");
		
		ckNullParaMap(req,check);
		
		HttpSession session=req.getSession();
		UserDto userDto=(UserDto)session.getAttribute("member");
		
		if (userDto==null) {
			throw new LoginException();
		}
		
		SellerDao dao=SellerDao.getInstance();
		
		String seller=userDto.getId();
		String status=req.getParameter("status");
		
		ProductDto product=new ProductDto();
		product.setSeller(seller);
		product.setStatus(status);
		
		return dao.getProductList(product);
	}
}

~~~

