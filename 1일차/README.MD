# 기본 UI 세팅
  - 기본 헤더 
![image](https://user-images.githubusercontent.com/106857769/186197425-e1195e3b-eeb1-47a1-9ac5-dd3e2119e00d.png)
  - 메뉴구성
![image](https://user-images.githubusercontent.com/106857769/186197557-4d2b50ac-0ac7-4709-b5e4-d308f7970b34.png)

# UI 코드
 - 기본 헤더 메뉴 ui 테그 구성
  ~~~html
<%@ tag body-content="scriptless" pageEncoding="utf-8"%>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts/"%>
<%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core" %>
<layout:common>
	
	<jsp:attribute name="header">
		<script src="<c:url value="/static/js/seller/common.js" />" ></script>
		<link rel="stylesheet" href="<c:url value="/static/css/seller/common.css" />" ></link>
		<header>
			
				<div>
					<a class="xi-library" id="menubtn"></a>
				</div>
				<div>
					<input id="search"><button id="searchbtn">검색</button>
				</div>
		
		</header>
		
		<nav class="menu dn">
			<ul >
				<a><li>제품 판매 신청</li></a>
				<a><li>신청 목록</li></a>
				<a><li>판매 목록</li></a>
			</ul>

		</nav>
		<div class="background dn">
		
		</div>
	</jsp:attribute>
	
	
	
	<jsp:body>
		<jsp:doBody />
	</jsp:body>
</layout:common>

  ~~~
  
  - 헤더 메뉴 자바스크립트
  
  ~~~html
    window.addEventListener("DOMContentLoaded",function(){
	const menubtnEl=document.getElementById("menubtn");
	const menuEl=document.getElementsByClassName("menu")[0];
	const menuBackEl=document.getElementsByClassName("background")[0];
	
	menubtnEl.addEventListener("click",function(){
		menuEl.className="menu";
		menuBackEl.className="background"
	})
	
	menuBackEl.addEventListener("click",function(){
		menuEl.className="menu dn";
		menuBackEl.className="background dn"
	})
})

  ~~~
  
# 메뉴 클릭시 이벤트처리
 ~~~
   menuEl.className="menu dn" div class="background dn"  
     메뉴 클릭 이벤트 발생! ▼  ▲ 메뉴 외의 어두운부분? 클릭시
   menuEl.className="menu" div class="background" 
 ~~~
 - 좌측 아이콘 클릭 시
 
  ![image](https://user-images.githubusercontent.com/106857769/186202725-3941b165-5c83-415f-8f7c-fdd0260cec17.png)
 
 - 메뉴 보이게 처리함
  
  ![image](https://user-images.githubusercontent.com/106857769/186203178-69979dff-efea-40da-a1cb-7f09d74ee40c.png)


  
  
