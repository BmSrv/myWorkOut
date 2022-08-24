# CreateAccountController 구성
  - GET : 로그인/회원가입 페이지로 이동
  - POST : 회원가입 처리 
~~~
package controllers.seller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import static jmsUtil.Utils.*;

import models.seller.CreateAccountService;
@WebServlet("/seller/createAccount")
public class CreateAccountController extends HttpServlet{
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String[] addCss=new String[] {"/seller/login"};
		String[] addJs=new String[] {"/seller/login"};
		req.setAttribute("addCss", addCss);
		req.setAttribute("addJs", addJs);
		RequestDispatcher rd=req.getRequestDispatcher("/jmsPage/login.jsp");
		rd.forward(req, resp);

	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		try {
			CreateAccountService service=new CreateAccountService();
			service.createAccount(req);
		} catch (Exception e) {
			e.printStackTrace();
			showAlertException(resp, e);
		}

	}
	
}

~~~

# CreateAccountService 구성
  - 값 null 체크
  ~~~
 ckNullParaMap(req);
  ~~~
  
  - ckNullParaMap(HttpServletRequest req) 구성
  ~~~
  	public static void ckNullParaMap(HttpServletRequest req) {

		Set parakey = req.getParameterMap().keySet();

		parakey.forEach(t -> {
			if (req.getParameter((String) t) == null || req.getParameter((String) t).isBlank()) {
				throw new RuntimeException(t + "값이 비어있습니다.");

			}
		});

	}
  ~~~
  
  - 값 받아오기
  ~~~
  String id = req.getParameter("id");
  String password = req.getParameter("password");
  String name = req.getParameter("passwordRe");
  String email = req.getParameter("email");
  String mobile = req.getParameter("mobile");
  String address = req.getParameter("address");
  
  UserDto user=new UserDto();
  user.setId(id);
  user.setPassword(password);
  user.setName(name);
  user.setEmail(email);
  user.setMobile(mobile);
  user.setAddress(address);
		
  ~~~
  
  - DB 에 유저 정보 추가
  ~~~
  UserDao dao=UserDao.getInstance();
  dao.insertSeller(user);
  ~~~
  
  - dao.insertSeller(UserDto user)
  ~~~
  	/**
	 * 유저(판매자) 정보 추가
	 * @param user
	 * @return
	 * @author 정민상
	 */
	public UserDto insertSeller(UserDto user) {
		SqlSession sqlsession = mybatis.Connection.getSession();
		int affectedRows = sqlsession.insert("userInfoMapper.insertSeller", user);
		
		sqlsession.commit();
		sqlsession.close();
		
		if(affectedRows < 1)
			return null;
		
		return user;
	}
  ~~~
  
  - userInfoMapper.insertSeller 설정
  
  ~~~
<!-- 유저(판매자) 정보 추가 -->
	<insert id="insertSeller" parameterType="dto.UserDto" useGeneratedKeys="true" keyProperty="num">
		INSERT INTO WorkOutUser (id, password, name, email, mobile, address, userType)
			VALUES(#{id}, #{password}, #{name}, #{email}, #{mobile}, #{address},'seller');
	</insert>	
  ~~~
  
# CreateAccountService 전반
~~~
package models.seller;

import javax.servlet.http.HttpServletRequest;

import dto.UserDto;
import models.member.UserDao;

import static jmsUtil.Utils.*;
public class CreateAccountService {
	public void createAccount(HttpServletRequest req) {
		showParaMap(req);
		
		ckNullParaMap(req);
		
		String id = req.getParameter("id");
		String password = req.getParameter("password");
		String name = req.getParameter("passwordRe");
		String email = req.getParameter("email");
		String mobile = req.getParameter("mobile");
		String address = req.getParameter("address");
		
		UserDto user=new UserDto();
		user.setId(id);
		user.setPassword(password);
		user.setName(name);
		user.setEmail(email);
		user.setMobile(mobile);
		user.setAddress(address);
		
		UserDao dao=UserDao.getInstance();
		dao.insertSeller(user);
		
	}
}

~~~

