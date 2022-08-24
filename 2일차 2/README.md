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
  ~~~


