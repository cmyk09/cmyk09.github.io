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
```