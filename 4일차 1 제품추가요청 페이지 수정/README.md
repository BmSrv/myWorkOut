#  제품 추가 요청 페이지

![image](https://user-images.githubusercontent.com/106857769/187015320-ee922d18-380e-4544-ab8d-df06a72d3da9.png)

# addProductReq.jsp
~~~
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts/sellertag" %>
<%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core" %>
<link rel="stylesheet" href="<c:url value="/static/css/seller/addProductReq.css" />" />

<layout:seller>
	<div class="background"></div>

	<div class="box">
	<h1>제품 추가 요청페이지 </h1>
	<form target="ifrmProcess" action="<c:url value="/seller/addProductReq" />" method="post" >
	
		<label for="serialnum">제품코드</label>
		 	<input type="text" id="serialnum" name="serialnum" class=""  autocomplete="off">
		
		<label for="name">제품명</label>
			<input type="text" id="name" name="name" class="" autocomplete="off">
				
		<label for="price">제품가격</label>
			<input type="text" id="price" name="price" class="" autocomplete="off">
		
		<label for="kategorie">카테고리</label>
			<input type="text" id="kategorie" name="kategorie" class="" autocomplete="off">
		
		<label for="company">브랜드</label>
			<input type="text" id="company" name="company" class="" autocomplete="off">
				
		<label for="img">이미지</label>
			<input type="file" id="img" name="img" class="" >
		<div class="btn-grp">
			<button>추가하기</button>
		</div>
		
	</form>
	</div>
</layout:seller>

~~~

# AddProductReqController.java
 - 요청이 정상적으로 이루어진 후 alert 와 메인페이지로 이동
~~~
package controllers.seller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import models.seller.AddProductService;
import static jmsUtil.Utils.*;

@WebServlet("/seller/addProductReq")
public class AddProductReqController extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		RequestDispatcher rd =req.getRequestDispatcher("/jmsPage/addProductReq.jsp");
		rd.forward(req, resp);
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		req.setCharacterEncoding("UTF-8");
		
		try {
			
			AddProductService service=new AddProductService();
			service.addProduct(req);
			showAlert(resp, "상품요청이 이루어졌습니다.");
			replacePage(resp, req.getContextPath()+"/seller", "parent");
			
		} catch (Exception e) {
			showAlertException(resp, e);
			e.printStackTrace();
		}
	}
}

~~~

# AddProductService
 - 미 로그인시 예외처리
 ~~~
 HttpSession session=req.getSession();
 UserDto user=(UserDto)session.getAttribute("member");
 if (user==null) {
   throw new RuntimeException("로그인 후 사용해 주세요");
 }
 ~~~
 
 - 요청 상품을 추가 할때 session 에서 로그인 정보를 가져와 추가하려는 상품(ProductDto.seller) 에 값을 추가한다(판매자의 id)
 ~~~
 	HttpSession session=req.getSession();
	UserDto user=(UserDto)session.getAttribute("member");
			(생략)
	ProductDto product=new ProductDto();
			(생략)
	product.setSeller(user.getId());
 ~~~
 
 - AddProductService 전반
 
 ~~~
package models.seller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import com.mysql.cj.Session;

import dto.UserDto;

public class AddProductService {
	public void addProduct(HttpServletRequest req) {
		/**
		 * 값받아오기
		 */
		HttpSession session=req.getSession();
		UserDto user=(UserDto)session.getAttribute("member");
		if (user==null) {
			throw new RuntimeException("로그인 후 사용해 주세요");
		}
		
		String serialnum =req.getParameter("serialnum");
		String name=req.getParameter("name");
		String price=req.getParameter("price");
		String kategorie=req.getParameter("kategorie");
		String company=req.getParameter("company");
		String img=req.getParameter("img");
		
		ProductDto product=new ProductDto();
		product.setSerialnum(serialnum);
		product.setName(name);
		product.setPrice(Integer.parseInt(price));
		product.setKategorie(kategorie);
		product.setCompany(company);
		product.setImg(img);
		product.setSeller(user.getId());
		SellerDao dao=SellerDao.getInstance();
		
		dao.register(product);
		
		
	}
}

 
 ~~~
 
