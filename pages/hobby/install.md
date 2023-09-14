---
title: Java
keywords: install
sidebar: hobby_sidebar
toc: false
permalink: install.html
folder: hobby
---
```java

Spring Framework   
    - 설치
      :  Help 메뉴 >  이클립스 마켓플레이스 에서 spring로 검색 > 스프링 툴 4 인스톨 > 컨펌 > 억셉트
======================================
Spring 프로젝트 추가시 설정
Type : Maven
java Version : 17
Packaging : jar
Language : java
Group : com.ezen
Artifact : 프로젝트 이름

> next 클릭
Developer Tools > Spring Boot DevTools
Web > Spring Web 체크

> next 클릭

Bass Url

Full Url 

> 완료

프로젝트 익스플로러에서
pom.xml 에서 설정을 확인 가능

Spring Boot 실행확인 : 익스플로러에서 Run As > Spring Boot app 클릭

src > main > resources > application.properties 
아래 
server.port=80
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
설정

WEB-INF 가 없는 경우
생성할것 main > webapp > WEB-INF  폴더를 생성

빌드패스 > Add Library 탭>클래스패스 Server Runtime 선택 > apply

빌드패스 > 좌측 메뉴에서 프로젝트 Facets > Dynamic Web Module 5.0 체크  > apply

좌측 프로젝트 익스플로러에서 마우스 우클릭 Spring > Upgrade Spring Version > 2.7 선택
=========================================
AJAX(jQuery)
<script src="https://code.jquery.com/jquery-3.7.0.min.js" integrity="sha256-2Pmvv0kuTBOenSvLm6bvfBSSHrUJ+3A7x6P5Ebd07/g=" crossorigin="anonymous"></script>
=========================================
JSON : (Javascript Standard Object Notation)  key 와 value 형식
 *    JSON Library 다운로드/설치/사용(json-simlpe.jar) jar:Java Archives(자바 표준 압축 포멧으로 zip 과 같음)
 *        : 순서 1) mavenrepository.com 에서 json.simple 검색/다운로드(버젼 1.1 에서 bundle 선택)       
 *              2) 자바프로젝트에 라이브러리 등록(Build path, 외부라리브러리 등록 - Libraries 탭 에서 'Add External JAERs' 로 추가)
 *                 안되면 Servlet>src>main>java>webapp>WEB-INF>lib 아래에 json-simple-1.1.1.jar 파일을 탐색기에서 복사 후 붙여넣기
 *              3) 소스코드에서 JSONObject 클래스 사용       
 *              4) 사용법 참조 (https://tychejin.tistory.com/139)  
==========================================
Oracle
JDBC 드라이버 다운로드
  https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html      
==========================================
EL, JSTL (jsp 에서만 사용이 가능한 java code를 없애는 방법)
EL(Expressinon Language) : $(scope object)
  : 주로 영역 오브젝트(Scope Object)에 저장된 데이터를 화면에 표시할 때 사용
    별도의 라이브러리가 필요치 않음  java 코드를 사용하지 않고 화면에 데이터 표시 가능
JSTL(Jsp Standard Tag Library)
  :jsp 페이지에서 자바 표준 태그를 사용하여 자바 코드를 대체할 수 있음
   별도의 라이브러리(jstl-1.2.jar 파일이 요구됨)
   Java 코드를 사용하지 않고 태그를 사용하여 Java 제어문을 사용할 수 있음

다운로드 싸이트 : 
   https://repo.maven.apache.org/maven2/jakarta/servlet/jsp/jstl/jakarta.servlet.jsp.jstl-api/2.0.0/jakarta.servlet.jsp.jstl-api-2.0.0.jar
   https://repo.maven.apache.org/maven2/org/glassfish/web/jakarta.servlet.jsp.jstl/2.0.0/jakarta.servlet.jsp.jstl-2.0.0.jar
=========================================
lombok 라이브러리 설정
  	1) 다운로드
  		: http://projectlombok.org/download  (1.18.28 다운로드)
  		  다운로드 후 cmd 창에서 해당 lombok.jar 을 실행한다.(cmd 에서 해당 파일을 찾아서 - cd 해서 해당 파일이 있는 폴더로 이동 - 
  		  											java -jar lombok.jar 하여 실행 후 이미 설치된 이클립스의 실행 파일을 선택하여
  		  											update/install 해준다.)
  		  http://mvn.repogitory.com 에서 lombok 로 검색하여 맞는 버전을 pom.xml에 추가한다.											
  	2) 사용
  	- VO 클래스에 @Data 를 선언하면 생성자, Setter/Getter, Override(toString/Equals, Hashcode) 등을 자동으로 생성해줌
  	- @Sl4j : log.info("askd") ; trace, warn, info, debug, error
  	- log Level : 로그 레벨을 지정하면 trace, warn, info, debug, error 들 중 필요한 것만 출력하도록 할 수 있음(프로그램 배포시 적절하게 사용)
=========================================
application.properties

server.port=80
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

#Database config
spring.datasource.url=jdbc:mysql://localhost:3306/ezen?characterEncoding=UTF-8&serverTimezone=UTC&SSL=false
spring.datasource.username=root
spring.datasource.password=ezenac
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

#mybatis pageHelper
pagehelper.helper-dialect=mysql
pagehelper.reasonable=true


## MULTIPART (MultipartProperties)
# Enable multipart uploads
spring.servlet.multipart.enabled=true
# Threshold after which files are written to disk.
spring.servlet.multipart.file-size-threshold=2KB
# Max file size.
spring.servlet.multipart.max-file-size=200MB
# Max Request Size
spring.servlet.multipart.max-request-size=215MB


# JPA
#spring.jpa.hibernate.ddl-auto=create
spring.jpa.hibernate.ddl-auto=none
spring.jpa.generate-ddl=false
spring.jpa.show-sql=true
spring.jpa.database=mysql
logging.level.org.hibernate=info
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect


# Thymeleaf
spring.thymeleaf.cache=false
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.view-names=ezen/*

# SMTP
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=cmyk9999@gmail.com
spring.mail.password=dxpgqwybvgegygds
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.auth=true
=================================
pom.xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.1.0</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.ezen</groupId>
	<artifactId>SpringProject</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>	
	<description>ezen project for Spring Boot</description>
	
	<properties>
		<jakarta-servlet.version>5.0.0</jakarta-servlet.version>
		<java.version>17</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<!-- 
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>
		--> 
		<dependency>
			<groupId>org.eclipse.jetty</groupId>
			<artifactId>apache-jsp</artifactId>
			
		</dependency>
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>jakarta.servlet.jsp.jstl</artifactId>
			<version>3.0.0</version>
		</dependency>
		
		<dependency>
          <groupId>com.mysql</groupId>
          <artifactId>mysql-connector-j</artifactId>
          <version>8.0.33</version>
      	</dependency>
      	<dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-jdbc</artifactId>
      	</dependency>
      	
      	<dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>3.0.0</version>
      	</dependency>
      	
      	<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper-spring-boot-starter -->
		<dependency>
		    <groupId>com.github.pagehelper</groupId>
		    <artifactId>pagehelper-spring-boot-starter</artifactId>
		    <version>1.4.6</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
		<dependency>
		    <groupId>org.projectlombok</groupId>
		    <artifactId>lombok</artifactId>
		    <version>1.18.28</version>
		    <scope>provided</scope>
		</dependency>
		
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-data-jpa</artifactId>
		    <version>3.1.1</version>
		</dependency>

  		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-thymeleaf -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-thymeleaf</artifactId>
		    <version>3.1.0</version>
		</dependency>

			
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
=============================
mapper.xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        				"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.ezen.goods.GoodsMgtMapper">

	<select id="getGoodsCategoryList1" 
			resultType="Map">
			SELECT distinct code_first as code, name_first as name 
			  FROM goods_category_list
			  order by 1
	</select>
	
	<select id="getGoodsCategoryList2" 
			parameterType="String"
			resultType="Map">
			SELECT distinct code_second as code, name_second as name 
			  FROM goods_category_list 
			 where code_first = #{key} 
			 order by 1
	</select>
	
	<select id="getGoodsCategoryList3" 
			parameterType="String"
			resultType="Map">
			SELECT distinct code_third as code, name_third as name 
			  FROM goods_category_list 
			 where code_second = #{key} 
			 order by 1
	</select>
	
	<insert id="addGoods"
			useGeneratedKeys="true"
			keyProperty="Goodsno"
			parameterType="com.ezen.goods.GoodsVO">
			INSERT INTO goods (Goodsno, CategoryCode, CategoryName, GoodsName, GoodsSaleQty, GoodsSalePrice, GoodsPrice, GoodsDetail, GoodsStatus, GoodsRegistryDay, SellerCode)
					VALUES (NULL, #{inputS3Code}, #{inputS3Name}, #{GoodsName}, #{GoodsSaleQty}, #{GoodsSalePrice}, #{GoodsPrice}, #{GoodsDetail}, #{GoodsStatus}, #{GoodsRegistryDay}, #{SellerCode})
	</insert>
	
	<insert id="saveGoodsImgFile"
			parameterType="Map" >
			INSERT INTO goods_img_list (Fileno, Goodsno, ImgFileName1, ImgFileFakeName1, ImgFileName2, ImgFileFakeName2, ImgFileName3, ImgFileFakeName3, ImgFileName4, ImgFileFakeName4, ImgFileName5, ImgFileFakeName5, ImgFileName6, ImgFileFakeName6) 
							    VALUES (NULL, #{Goodsno}, #{imgName1}, #{imgFakeName1}, #{imgName2},   #{imgFakeName2},  #{imgName3},  #{imgFakeName3},  #{imgName4},  #{imgFakeName4},  #{imgName5},  #{imgFakeName5},  #{imgName6},  #{imgFakeName6})
			
	</insert>
	
	<select id="detailGoodsForm" 
			parameterType="Integer"
			resultType="Map">
			SELECT g.Goodsno, CategoryCode, CategoryName, 
					GoodsName, GoodsSaleQty, GoodsSalePrice,
					GoodsPrice, GoodsDetail, GoodsRegistryDay,
					case when ImgFileName1 is null then 'X' 
									 when ImgFileName1 = '' then 'X' 
									 else ImgFileName1 end ImgFileName1,
			                    case when ImgFileFakeName1 is null then 'X' 
			                    	 when ImgFileFakeName1 = '' then 'X'
			                    	 else ImgFileFakeName1 end ImgFileFakeName1,
								case when ImgFileName2 is null then 'X' 
									 when ImgFileName2 = '' then 'X' 
									 else ImgFileName2 end ImgFileName2,
			                    case when ImgFileFakeName2 is null then 'X' 
			                    	 when ImgFileFakeName2 = '' then 'X' 
			                    	 else ImgFileFakeName2 end ImgFileFakeName2,
			                    case when ImgFileName3 is null then 'X' 
			                    	 when ImgFileName3 = '' then 'X' 
			                   	  	 else ImgFileName3 end ImgFileName3,
			                    case when ImgFileFakeName3 is null then 'X' 
			                    	 when ImgFileFakeName3 = '' then 'X'
			                    	 else ImgFileFakeName3 end ImgFileFakeName3,
			                    case when ImgFileName4 is null then 'X' 
			                    	 when ImgFileName4 = '' then 'X' 
			                    	 else ImgFileName4 end ImgFileName4,
			                    case when ImgFileFakeName4 is null then 'X' 
			                    	 when ImgFileFakeName4 = '' then 'X' 
			                    	 else ImgFileFakeName4 end ImgFileFakeName4,
			                    case when ImgFileName5 is null then 'X' 
			                    	 when ImgFileName5 = '' then 'X' 
			                    	 else ImgFileName5 end ImgFileName5,
			                    case when ImgFileFakeName5 is null then 'X' 
			                    	 when ImgFileFakeName5 = '' then 'X'
			                    	 else ImgFileFakeName5 end ImgFileFakeName5,
			                    case when ImgFileName6 is null then 'X' 
			                    	 when ImgFileName6 = '' then 'X' 
			                    	 else ImgFileName6 end ImgFileName6,
			                    case when ImgFileFakeName6 is null then 'X' 
			                    	 when ImgFileFakeName6 = '' then 'X' 
			                    	 else ImgFileFakeName6 end ImgFileFakeName6
  			FROM (select * from goods where Goodsno = #{Goodsno}) g left outer join goods_img_list i
    		  ON g.goodsno = i.goodsno 
	</select>
	
	<select id="updateGoodsForm" 
			parameterType="Integer"
			resultType="Map">
			Select * 
			from
			( SELECT g.Goodsno, CategoryCode, CategoryName, 
								GoodsName, GoodsSaleQty, GoodsSalePrice,
								GoodsPrice, GoodsDetail, GoodsRegistryDay,
								case when ImgFileName1 is null then 'X' 
									 when ImgFileName1 = '' then 'X' 
									 else ImgFileName1 end ImgFileName1,
			                    case when ImgFileFakeName1 is null then 'X' 
			                    	 when ImgFileFakeName1 = '' then 'X'
			                    	 else ImgFileFakeName1 end ImgFileFakeName1,
								case when ImgFileName2 is null then 'X' 
									 when ImgFileName2 = '' then 'X' 
									 else ImgFileName2 end ImgFileName2,
			                    case when ImgFileFakeName2 is null then 'X' 
			                    	 when ImgFileFakeName2 = '' then 'X' 
			                    	 else ImgFileFakeName2 end ImgFileFakeName2,
			                    case when ImgFileName3 is null then 'X' 
			                    	 when ImgFileName3 = '' then 'X' 
			                   	  	 else ImgFileName3 end ImgFileName3,
			                    case when ImgFileFakeName3 is null then 'X' 
			                    	 when ImgFileFakeName3 = '' then 'X'
			                    	 else ImgFileFakeName3 end ImgFileFakeName3,
			                    case when ImgFileName4 is null then 'X' 
			                    	 when ImgFileName4 = '' then 'X' 
			                    	 else ImgFileName4 end ImgFileName4,
			                    case when ImgFileFakeName4 is null then 'X' 
			                    	 when ImgFileFakeName4 = '' then 'X' 
			                    	 else ImgFileFakeName4 end ImgFileFakeName4,
			                    case when ImgFileName5 is null then 'X' 
			                    	 when ImgFileName5 = '' then 'X' 
			                    	 else ImgFileName5 end ImgFileName5,
			                    case when ImgFileFakeName5 is null then 'X' 
			                    	 when ImgFileFakeName5 = '' then 'X'
			                    	 else ImgFileFakeName5 end ImgFileFakeName5,
			                    case when ImgFileName6 is null then 'X' 
			                    	 when ImgFileName6 = '' then 'X' 
			                    	 else ImgFileName6 end ImgFileName6,
			                    case when ImgFileFakeName6 is null then 'X' 
			                    	 when ImgFileFakeName6 = '' then 'X' 
			                    	 else ImgFileFakeName6 end ImgFileFakeName6
			  			FROM (select * from goods where Goodsno = #{Goodsno}) g left outer join goods_img_list i
			    		  ON g.goodsno = i.goodsno 
			) d left outer join goods_category_list l
			on d.CategoryCode = l.code_third
	</select>
	
	<update id="updateGoods"
			parameterType="com.ezen.goods.GoodsVO">
			UPDATE  goods 
			   SET  CategoryCode = #{inputS3Code} ,
					CategoryName = #{inputS3Name} ,
					GoodsName = #{GoodsName} ,
					GoodsSaleQty = #{GoodsSaleQty} ,
					GoodsSalePrice = #{GoodsSalePrice} ,
					GoodsPrice = #{GoodsPrice} ,
					GoodsDetail = #{GoodsDetail} 
	 		 WHERE Goodsno = #{Goodsno}
	</update>
	
	<update id="updateGoodsImgFile"
			parameterType="Map">
			UPDATE  goods_img_list 
			   SET  ImgFileName1 = #{ImgFileName1}, 
					ImgFileFakeName1 = #{ImgFileFakeName1}, 
					ImgFileName2 = #{ImgFileName2}, 
					ImgFileFakeName2 = #{ImgFileFakeName2}, 
					ImgFileName3 = #{ImgFileName3},
					ImgFileFakeName3 = #{ImgFileFakeName3}, 
					ImgFileName4 = #{ImgFileName4},
					ImgFileFakeName4 = #{ImgFileFakeName4}, 
					ImgFileName5 = #{ImgFileName5},
					ImgFileFakeName5 = #{ImgFileFakeName5}, 
					ImgFileName6 = #{ImgFileName6},
					ImgFileFakeName6 = #{ImgFileFakeName6}
	 		 WHERE Goodsno = #{Goodsno}
	</update>
	
	<update id="deleteGoodsImgFile"
			parameterType="Map">
			UPDATE  goods_img_list 
			<choose>
				<when test = "fileno == 1 ">
					SET  ImgFileName1 = '',
						ImgFileFakeName1 = '' 
				</when>
				<when test = "fileno == 2 ">
					SET  ImgFileName2 = '',
						ImgFileFakeName2 = '' 
				</when>
				<when test = "fileno == 3 ">
					SET  ImgFileName3 = '',
						ImgFileFakeName3 = '' 
				</when>
				<when test = "fileno == 4 ">
					SET  ImgFileName4 = '',
						ImgFileFakeName4 = '' 
				</when>
				<when test = "fileno == 5 ">
					SET  ImgFileName5 = '',
						ImgFileFakeName5 = '' 
				</when>
				<when test = "fileno == 6 ">
					SET  ImgFileName6 = '',
						ImgFileFakeName6 = '' 
				</when>
			</choose>
	 		 WHERE Goodsno = #{goodsno}
	</update>
	
	<insert id="putInCart"
			parameterType="com.ezen.goods.CartVO" >
			INSERT INTO cart (cartno, userno, goodsno, goodsSalePrice, goodsSaleQty, cartRegistryDay)  
			 VALUES (NULL, #{userno}, #{goodsno}, #{goodsSalePrice}, #{goodsSaleQty},   #{cartRegistryDay})
			
	</insert>
	
	<select id="getCartList" 
			parameterType="Integer"
			resultType="Map">
			select d.cartno, d.userno, d.goodsno, i.imgFileFakeName1, d.goodsname, d.goodsSalePrice, d.goodsSaleQty, d.cartRegistryDay
			 from (
					select cartno, userno, c.goodsno, g.GoodsName, c.goodsSalePrice, c.goodsSaleQty, c.cartRegistryDay
					  from cart c left outer join goods g 
 					   on c.goodsno = g.goodsno
 					 where c.userno = #{userno}
  					) d left outer join goods_img_list i
 				 on d.goodsno = i.goodsno
 			 order by d.cartRegistryDay
	</select>
	
	<select id="getOrderNo" 
			resultType="Integer">
			select case when max(orderNo) is null then 0 
						else max(orderNo) end orderNo
 			 from orderlist
	</select>
	
	<select id="getCartOrderList" 
			parameterType="Map"
			resultType="com.ezen.goods.OrderVO">
			SELECT  userno as memberno, c.goodsno, g.SellerCode as sellerno, c.goodsSalePrice, c.goodsSaleQty
			 FROM cart c left outer join goods g
			  on c.goodsno = g.goodsno
			where userno = #{userno}
			  and cartno = #{cartno}
	</select>
	
	<insert id="addOrder"
			parameterType="com.ezen.goods.OrderVO" >
			INSERT INTO orderlist (num, orderNo, memberNo, goodsNo, sellerNo, goodsSalePrice, goodsSaleQty, orderRegistryDay, OrderStatus) VALUES
			<foreach collection="list" item="item" index="index" separator=",">
				(NULL,#{item.orderno},#{item.memberno},#{item.goodsno},#{item.sellerno},#{item.goodsSalePrice},#{item.goodsSaleQty},#{item.orderRegistryDay},#{item.status} )
			</foreach>
	</insert>
	
	<delete id="deletedCart"
			parameterType="com.ezen.goods.CartVO">
			DELETE FROM cart 
			WHERE cartno = #{cartno}
	</delete>
	
	<select id="getOrderListMember" 
			parameterType="Integer"
			resultType="Map">
			SELECT num, orderNo, memberNo, o.goodsNo, g.GoodsName, o.goodsSalePrice, o.goodsSaleQty, orderRegistryDay, OrderStatus
			 FROM orderList  o left outer join goods g
			  on o.goodsNo = g.goodsNo
			where o.memberno = #{userno}
	</select>
	
	<select id="getGoodsCategoryListAll" 
			resultType="Map">
			SELECT * FROM goods_category_list
	</select>
	
	<select id="getGoodsMain" 
			resultType="Map">
			SELECT g.Goodsno, g.GoodsName, img.ImgFileFakeName1, GoodsSalePrice, GoodsPrice
 			FROM goods g left outer join goods_img_list img
   			on g.Goodsno = img.Goodsno
   			where goodsStatus = 'S'
 			ORDER BY RAND() limit 8
	</select>
	
	<select id="getGoodsSearchKeyword" 
			parameterType="String"
			resultType="Map">
			SELECT g.Goodsno, g.GoodsName, img.ImgFileFakeName1, GoodsSalePrice, GoodsPrice, g.GoodsRegistryDay
			FROM goods g left outer join goods_img_list img
			on g.Goodsno = img.Goodsno
			where g.GoodsStatus = 'S'
			  and g.GoodsName like #{keyword}
			ORDER BY g.GoodsRegistryDay desc
	</select>
	
	<select id="getGoodsSearchCategory" 
			parameterType="Map"
			resultType="Map">
			SELECT g.Goodsno, g.GoodsName, img.ImgFileFakeName1, GoodsSalePrice, GoodsPrice, g.GoodsRegistryDay
			  FROM (select * 
					from goods
					where GoodsStatus = 'S'
					and categorycode in (  
										select code_third
										from goods_category_list
										where substring(code_third , 1, 2 * #{lvl}) = #{code})) g left outer join goods_img_list img
			   on g.Goodsno = img.Goodsno
			ORDER BY g.GoodsRegistryDay desc
	</select>
	
	<update id="updateGoodsQty"
			parameterType="Map">
			UPDATE  goods
			   SET  goodsSaleQty = goodsSaleQty - #{goodsSaleQty}
	 		 WHERE  Goodsno = #{goodsNo}
	</update>

	<select id="goodsList" 
			parameterType="Integer"
			resultType="Map">
			SELECT * 
			FROM goods 
			where GoodsStatus in ('I', 'S', 'E')
			  and SellerCode =  #{sellercode}
			order by goodsno
	</select>
	
	<update id="endSale"
			parameterType="Integer">
			UPDATE  goods
			   SET  goodsStatus = 'E'
	 		 WHERE  Goodsno = #{goodsNo}
	</update>
	
	<update id="startSale"
			parameterType="Integer">
			UPDATE  goods
			   SET  goodsStatus = 'S'
	 		 WHERE  Goodsno = #{goodsNo}
	</update>
	
	<update id="changeSaleQty"
			parameterType="Map">
			UPDATE  cart
			   SET  goodsSaleQty = #{goodsSaleQty}
	 		 WHERE  cartno = #{cartno}
	</update>
	
	<update id="deleteGoods"
			parameterType="Map">
			UPDATE  goods
			   SET  GoodsStatus = 'D',
			   		GoodsDeleteDay = #{deleteDay} 
	 		 WHERE Goodsno = #{goodsno}
	</update>
	
	<update id="purConfirm" parameterType="com.ezen.goods.OrderVO">
		update orderlist set orderstatus='purConfirm' where orderNo=#{orderNo}
	</update>
</mapper>
```