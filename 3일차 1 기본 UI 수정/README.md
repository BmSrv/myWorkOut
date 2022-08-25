# 기본 UI 수정
![image](https://user-images.githubusercontent.com/106857769/186710638-5723a685-ef9b-4ee9-b586-f7e034249435.png)

![image](https://user-images.githubusercontent.com/106857769/186711012-a9ab83dc-0f75-4083-b56f-a5833ae128ad.png)

# seller 테그 
~~~
<%@ tag body-content="scriptless" pageEncoding="utf-8"%>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts/"%>
<%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core" %>
<layout:common>
	
	<jsp:attribute name="header">
		<script src="<c:url value="/static/js/seller/common.js" />" ></script>
		<link rel="stylesheet" href="<c:url value="/static/css/seller/common.css" />" ></link>
		<header>
			<nav class="header_nav">
				<c:if test="${!empty member }">
				<ul>
					<a href="<c:url value="/seller" />">
						<li>
							<div class="xi-profile-o header_icon" ></div>
							<div class="info">${member.name } </div>
						</li>
					</a>
				</c:if>
				<c:if test="${empty member }">
				<ul>
					<a href="<c:url value="/seller/login" />">
						<li>
							<div class="xi-profile-o header_icon" ></div>
							<div class="info">로그인 </div>
						</li>
					</a>
				</c:if>
				
					<a href="<c:url value="/seller" />">
						<li>
							<div class="xi-home-o header_icon" ></div>
							<div class="info">메인페이지</div>
						</li>
					</a>
				
					<a href="">
						<li>
							<div class="xi-message-o header_icon" ></div>
							<div class="info">메세지</div>
						</li>
					</a>

					<a href="<c:url value="/seller/addProductReq" />" >
						<li>
							<div class="xi-cart-add header_icon" /></div>
							<div class="info">상품 추가 요청</div>
						</li>
					</a>

					<a href="">
						<li>
							<div class="xi-shop header_icon" /></div>
							<div class="info">판매중인 상품</div>
						</li>
					</a>
					
					<c:if test="${!empty member }">
				<ul>
					<a href="<c:url value="/seller" />">
						<li>
							<div class="xi-log-out header_icon" ></div>
							<div class="info">로그아웃 </div>
						</li>
					</a>
				</c:if>

				</ul>
			</nav>
		</header>
	
	</jsp:attribute>
	
	
	
	<jsp:body>
		<div class="main">
			<jsp:doBody />
		</div>
	</jsp:body>
</layout:common>
~~~
