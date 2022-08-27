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
