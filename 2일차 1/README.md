# 판매자 로그인 페이지 구성

![image](https://user-images.githubusercontent.com/106857769/186479725-a676ea1a-fb42-4b27-b801-8534dca45682.png)


# 판매자 회원가입 페이지 구성

![image](https://user-images.githubusercontent.com/106857769/186483567-3fce1beb-7890-4c84-a59c-a96e49ba9b54.png)

# jsp 코드
~~~
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="layout" tagdir="/WEB-INF/tags/layouts" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<layout:common>
<!-- 로그인 S -->
	<div class="loginArea">
	<form>
		<dl>
			<a class="xi-user-o loginlogo"></a>
			<dt class="message"><h2>Please sign in</h2></dt>
			<dd class="input-grp" >
				<input placeholder="User ID">
				<input placeholder="Password">
			</dd>
		</dl>
		
		<div class="chk-grp">
			<input type="checkbox" id="remember">
			<label for="remember">Remember me</label>
		</div>
		
		<div class="btn-grp">
			<button>Sing in</button>
			<button type="button" id="showReg">Register</button>
		</div>
		
		</form>
	</div>
	<!-- 로그인 E -->
	
	<!-- 회원가입 S -->
	<div class="RegisterArea dn">
	<form action="<c:url value="/seller/createAccount" />" method="post">
		<dl>
		<!-- num, id, password, name, email, mobile, address, userType -->
			<a class="xi-user-o loginlogo"></a>
			
			<dt class="message">
				<h2>Create Account</h2>
			</dt>
			<div class="input-outer">			
				<dd class="input-grp" >
					<input placeholder="User ID" name="id">
					<input placeholder="Password" name="password">
					<input placeholder="PasswordRe" name="passwordRe">
					<input placeholder="Group">
					
				</dd>
			
				<dd class="input-grp" >
					<input placeholder="name" name="name">
					<input placeholder="email" name="email">
					<input placeholder="mobile" name="mobile">
					<input placeholder="address" name="address">
				</dd>
			<div>			
			
		</dl>
		
		<!-- <div class="chk-grp">
			<input type="checkbox" id="remember">
			<label for="remember">Remember me</label>
		</div> -->
		
		<div class="btn-grp">
			<button type="button" id="showSign">Sing in</button>
			<button>Register</button>
		</div>
		
		</form>
	</div>
		<!-- 회원가입 E -->
	
</layout:common>
~~~

# 자바스크립트 코드
~~~
window.addEventListener("DOMContentLoaded",function(){
	const showSignEl=document.getElementById("showSign");
	const showRegEl=document.getElementById("showReg");
	const RegisterAreaEl=document.getElementsByClassName("RegisterArea")[0];
	const loginAreaEl=document.getElementsByClassName("loginArea")[0];
	
	showSignEl.addEventListener("click",function(){
		loginAreaEl.className="loginArea ";
		RegisterAreaEl.className="RegisterArea dn";
	});
	
	showRegEl.addEventListener("click",function(){
		loginAreaEl.className="loginArea dn";
		RegisterAreaEl.className="RegisterArea";
	})
	
})
~~~
  - 로그인 창에서 회원가입 창으로 전환
   ~~~
   showSignEl.addEventListener("click",function(){
		loginAreaEl.className="loginArea ";
		RegisterAreaEl.className="RegisterArea dn";
	});
   ~~~
  - 회원가입 창에서 로그인 창으로 전환
  ~~~
   showRegEl.addEventListener("click",function(){
		loginAreaEl.className="loginArea dn";
		RegisterAreaEl.className="RegisterArea";
	})
   ~~~
