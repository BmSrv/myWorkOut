# 제품 추가 요청 페이지

![image](https://user-images.githubusercontent.com/106857769/186224815-98e0757c-ef4e-4bcf-b9bd-77f449927488.png)

# 제품 추가 페이지 컨트롤러
  - GET  : jsp 페이지 로딩
  - POST : 제품 추가 DB 처리 등
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
			
		} catch (Exception e) {
			showAlertException(resp, e);
		}
	}
}

~~~

# mybatis productMapper.xml
~~~
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="ProductMap">
	<resultMap type="controllers.seller.ProductDto" id="productmap">
		<result column="abnum" property="abnum"/>
		<result column="serialnum" property="serialnum"/>
		<result column="name" property="name"/>
		<result column="price" property="price"/>
		<result column="kategorie" property="kategorie"/>
		<result column="company" property="company"/>
		<result column="img" property="img"/>
	</resultMap>

</mapper>
~~~

# ProductDto 
~~~

public class ProductDto {
	private int abnum;
	private String serialnum;
	private String name;
	private int price;
	private String kategorie;
	private String company;
	private String img;
	
	public ProductDto() {}

	public int getAbnum() {
		return abnum;
	}

	public void setAbnum(int abnum) {
		this.abnum = abnum;
	}

	public String getSerialnum() {
		return serialnum;
	}

	public void setSerialnum(String serialnum) {
		this.serialnum = serialnum;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	public String getKategorie() {
		return kategorie;
	}

	public void setKategorie(String kategorie) {
		this.kategorie = kategorie;
	}

	public String getCompany() {
		return company;
	}

	public void setCompany(String company) {
		this.company = company;
	}

	public String getImg() {
		return img;
	}

	public void setImg(String img) {
		this.img = img;
	}

	@Override
	public String toString() {
		return "ProductDto [abnum=" + abnum + ", serialnum=" + serialnum + ", name=" + name + ", price=" + price
				+ ", kategorie=" + kategorie + ", company=" + company + ", img=" + img + "]";
	}
	
	
	
	
}
~~~

# SellerDao 세팅 (싱글톤)
~~~
package models.seller;

public class SellerDao {
	private static SellerDao instance=new SellerDao();
	private SellerDao() {}
	
	
	
	public static SellerDao getInstance() {
		if (instance==null) {
			instance=new SellerDao();
		}
		return instance;
	}
}

~~~







