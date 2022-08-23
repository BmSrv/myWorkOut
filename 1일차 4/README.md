# AddProductService 구성

 - 값 받아오기
 ~~~html
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
		
		
 ~~~
 
 - DB 에 값 저장
 ~~~
SellerDao dao=SellerDao.getInstance();

dao.register(product);
 ~~~
 
 - dao.register(ProductDto productDto) 구성
 ~~~
public void register(ProductDto product) {
  SqlSession session=Connection.getSession();
		
  session.insert("RequestProductMap.register",product);
		
  session.commit();
  session.close();
  System.out.println(product);
		
	}
 ~~~
 -mybatis Mapper 구성
 ~~~
 <insert id="register"  parameterType="models.seller.ProductDto" useGeneratedKeys="true"
        keyProperty="abnum" keyColumn="abnum">
	INSERT RequestProduct ( serialnum, name, price, kategorie, company, img) 
		VALUES (#{serialnum},#{name},#{price},#{kategorie},#{company},#{img})
</insert>
 ~~~
 
