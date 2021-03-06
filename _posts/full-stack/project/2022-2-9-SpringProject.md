---
title: 스프링 프로젝트
category: project_fullstack
---

# 환경설정

## 필요파일

- `ojdbc6.jar`
- `apache-tomcat-8.5.73` 로컬서버 실행
- `log4jdbc.log4j2.properties` 

**[workspace]**

인코딩 (enc): UTF-8

**[프로젝트 생성]**

프로젝트: Spring Legacy Project

템플릿: Spring MVC Project

**[web.xml]**

```xml
<!-- 인코딩 -->
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <servlet-name>appServlet</servlet-name>
</filter-mapping>
```

**[pom.xml]**

java-version: 1.8

springframework-version: 5.0.7

**Spring&MyBatis**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>${org.springframework-version}</version>
</dependency>

<!-- 마이바티스 -->
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>2.7.8</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>
<dependency>
    <groupId>org.bgee.log4jdbc-log4j2</groupId>
    <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
    <version>1.16</version>
</dependency>

<!-- 롬복 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.0</version>
    <scope>provided</scope>
</dependency>        
```

**테스트**

junit: 4.12

**Servlet**

javax.servlet-api: 3.1.0

**Maven**

maven-compiler-plugin

- source: 1.8
- target: 1.8

**logging**

`<scope>runtime</scope>` 주석처리해야 log4j 를 사용할 수 있는듯하다.

1. Update Project
2. Build Path 에 ojdbc6.jar 추가
3. Deployment Assembly 에도 추가

JRE System Library: JavaSE-1.8

Project Facets: Java 1.8

**[서버]**

Apache Tomcat v8.5 로 생성

경로 `/` 로 변경

**[SQL Developer]**

```sql
CREATE USER board
IDENTIFIED BY tiger;

GRANT CONNECT, RESOURCE
TO board;

create sequence seq_board;

create table tbl_board (
  bno number(10,0),
  title varchar2(200) not null,
  content varchar2(2000) not null,
  writer varchar2(50) not null,
  regdate date default sysdate, 
  updatedate date default sysdate
);

alter table tbl_board add constraint pk_board 
primary key (bno);

BEGIN
    FOR i IN 1..30
    LOOP
        INSERT INTO tbl_board (bno, title, content, writer)
        VALUES (seq_board.nextval, '공지사항' || i, '공지사항 내용' || i, '관리자' || i);
    END LOOP;
    COMMIT;        
END;
```

# 에러

## 브랜치 이동

브랜치 이동 후 오류가 난다면 프로젝트 우클릭 - refresh - 서버실행

## Request method 'POST' not supported

get 방식은 잘 되는데 post 방식에서 에러가 난다면

security 설정 후 에러가 발생한다면 

CSRF 를 생각해보자.

```java
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
```

form 태그에 이 코드를 작성하면 된다.

# 시작

## DB연결

**DB연결 정리**

1. root-context.xml
2. properties
3. 객체
4. Mapper 인터페이스
6. (선택) Mapper.xml

**root-context.xml**

```xml
<bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
    <property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
    <property name="jdbcUrl" value="jdbc:log4jdbc:oracle:thin:@localhost:1521:xe"></property>
    <property name="username" value="아이디"></property>
    <property name="password" value="비밀번호"></property>
</bean>
<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close">
    <constructor-arg ref="hikariConfig"></constructor-arg>
</bean>	
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <property name="typeAliases">
        <list> 
            <value>패키지명.domain.BoardVO</value>
        </list>
    </property>		        
</bean>
<mybatis-spring:scan base-package="패키지명.mapper"/>	
```

**예**

- 아이디: SQL 계정 아이디
- 비밀번호: SQL 계정 비밀번호
- 패키지명: io.github.dev_connor.mapper 

**log4jdbc.log4j2.properties**

```
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
```

이 파일을 src/main/resources 에 root-context.xml 코드와 함께 추가한다.

**데이터 객체**

```java
@Data
public class BoardVO {
	private long bno;
	private String title; 
	private String content;
	private String writer;
	private Date regdate; 
	private Date updateDate;
}
```

**Mapper 인터페이스**

```java
public interface BoardMapper {
	@Select("SELECT * FROM tbl_board WHERE bno > 0")
	public List<BoardVO> getList();
}
```

**테스트**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class BoardMapperTests {
    
   @Setter(onMethod_ = @Autowired)
   private BoardMapper mapper;

   @Test
   public void testGetList() {
      mapper.getList().forEach(board -> log.info(board));
   } 
}
```

**Mapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="패키지명.mapper.BoardMapper">

   <select id="getList" resultType="BoardVO">
      <![CDATA[
      select * from tbl_board where bno > 0 
      ]]>
   </select>
</mapper>
```

**예**

- 패키지명: io.github.dev_connor

## 컨트롤러 구현

**정리**

1. controller
2. service
3. jsp

### 글 목록

**controller**

```java
@Controller
@Log4j
@RequestMapping("/board/*")
@AllArgsConstructor
public class BoardController {
   private static final Logger logger = LoggerFactory.getLogger(BoardController.class);
   private  BoardService service;

   @GetMapping("/list")
   public void list(Model model) {
      log.info("list");
      model.addAttribute("list", service.getList());
   }
} 
```

**service 인터페이스**

```java
public interface BoardService {
   public List<BoardVO> getList();
}
```

**service**

```java
@Log4j
@Service
@AllArgsConstructor
public class BoardServiceImpl implements BoardService {

   @Setter(onMethod_ = @Autowired )
   private BoardMapper mapper;
    
    @Override
    public List<BoardVO> getList() {   
       log.info("getList..........");   
       return mapper.getList();
    }
}
```

**jsp**

```jsp
<%@ page contentType="text/html; charset=UTF-8"   pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
   
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

   <h1>List Page</h1>

   <br>
   <a href="/board/register">register</a>
   <br>
   <br>

   <table class="table table-striped table-bordered table-hover">
      <thead>
         <tr>
            <th>#번호</th>
            <th>제목</th>
            <th>작성자</th>
            <th>작성일</th>
            <th>수정일</th>
         </tr>
      </thead>

      <c:forEach items="${list}" var="board">
         <tr>
            <td><c:out value="${board.bno}" /></td>
            
            
<%--             <td><a href='/board/get?bno=<c:out value="${board.bno}"/>'><c:out --%>
<%--                      value="${board.title}" /></a></td> --%>

            <td>
            	<a class='move' href='<c:out value="${board.bno}"/>'> 
            		<c:out value="${board.title}" />
            	</a>
            </td> 

            <td><c:out value="${board.writer}" /></td>
            <td><fmt:formatDate pattern="yyyy-MM-dd"
                  value="${board.regdate}" /></td>
            <td><fmt:formatDate pattern="yyyy-MM-dd"
                  value="${board.updateDate}" /></td>
         </tr>
      </c:forEach>
   </table>
   
   <!-- p 311 -->
		<form id='actionForm' action="/board/list" method='get'>
			<input type='hidden' name='pageNum' value='${pageMaker.cri.pageNum}'>
			<input type='hidden' name='amount' value='${pageMaker.cri.amount}'>
			<input type='hidden' name='type'
				value='<c:out value="${ pageMaker.cri.type }"/>'> 
			<input type='hidden' name='keyword'
				value='<c:out value="${ pageMaker.cri.keyword }"/>'>
		</form>
   
   
	   <!-- p 308 -->
	   <div class='pull-right'>
	      <ul class="pagination"> 
	          <c:if test="${pageMaker.prev}">
	              <li class="paginate_button previous"><a href="${pageMaker.startPage - 1 }">Previous</a>
	              </li>
	            </c:if> 
	            <c:forEach var="num" begin="${pageMaker.startPage}"
	              end="${pageMaker.endPage}">
	              <li class="paginate_button"><a href="${num }">${num}</a></li>
	            </c:forEach>
	            <c:if test="${pageMaker.next}">
	              <li class="paginate_button next"><a href="${pageMaker.endPage + 1 }">Next</a></li>
	            </c:if>  
	      </ul>
	   </div>
	   <!--  end Pagination -->   

   <br>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
   $(document).ready(function(){
      var result = '<c:out value="${result}"/>';
      
      checkModal(result);
      
      // p.257
      history.replaceState({}, null, null);
      
      function checkModal(result){
         if(result === '' || history.state) return;
         if( parseInt(result)>0 ) alert("게시글 " + parseInt(result) +" 번이 등록되었습니다. "); 
      }
      
      	// 페이징처리
		var actionForm = $("#actionForm");

		$(".paginate_button a").on(
				"click",
				function(e) {

					e.preventDefault();

					console.log('click');

					actionForm.find("input[name='pageNum']")
							.val($(this).attr("href"));
					actionForm.submit();
				});

		$(".move").on("click",function(e) {
			e.preventDefault();
			actionForm.append("<input type='hidden' name='bno' value='"
				+ $(this).attr("href") + "'>");
			actionForm.attr("action", "/board/get");
			actionForm.submit();
		});

		var searchForm = $("#searchForm");

		$("#searchForm button").on(
				"click",
				function(e) {

					if (!searchForm.find("option:selected")
							.val()) {
						alert("검색종류를 선택하세요");
						return false;
					}

					if (!searchForm.find(
							"input[name='keyword']").val()) {
						alert("키워드를 입력하세요");
						return false;
					}

					searchForm.find("input[name='pageNum']")
							.val("1");
					e.preventDefault();

					searchForm.submit();

				});
   });
</script>

</body>
</html>
```

### 글 상세

**controller**

```java
@GetMapping({ "/get", "/modify" })
public void get(@RequestParam("bno") Long bno, @ModelAttribute("cri") Criteria cri, Model model) {
    log.info("/get or modify");
    model.addAttribute("board", service.get(bno));
}
```

**service**

```java
@Override
public BoardVO get(Long bno) {
    log.info("get......" + bno);
    return mapper.read(bno);
}
```

**Mapper.xml**

```xml
<select id="read" resultType="BoardVO">
    select * 
    from tbl_board 
    where bno =   #{bno}
</select>   
```

### 글 작성

```xml
<insert id="insertSelectKey">
    <selectKey keyProperty="bno" order="BEFORE"   resultType="long">
        select seq_board.nextval from dual
    </selectKey>
    insert into tbl_board (bno,title,content, writer)
    values (#{bno}, #{title}, #{content}, #{writer})
</insert>    
```

### 글 수정

```xml
<update id="update">
    update tbl_board
    set title= #{title}, content=#{content}, writer=#{writer}, updateDate=sysdate
    where bno= #{bno}
</update>   
```

### 글 삭제

```xml
<delete id="delete">
    delete tbl_board 
    where bno = #{bno}
</delete>   
```

## CSS 적용

**list.jsp**

```jsp
<link href="${webappRoot}/resources/css/board.css" type="text/css" rel="stylesheet" />
```

**board.css**

webapp/resources/css 폴더 내에 작성한다.

```css
.paginate_button {
	list-style: none;
	float: left;
	padding: 3px;
}
```

## 로그인

**정리**

- pom.xml
- security-context.xml
- web.xml 에 security 필터 추가
- controller 와 JSP 구현

**pom.xml**

```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-core</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-taglibs</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
```

spring 폴더에 Spring Bean Configuration File 생성

**security-context.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:security="http://www.springframework.org/schema/security"
	xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
		
	<security:http>
		<security:intercept-url pattern="/sample/all"
			access="permitAll" />
		<security:intercept-url
			pattern="/sample/member" access="hasRole('ROLE_MEMBER')" />
		<security:form-login/>
	</security:http>

	<security:authentication-manager>
	</security:authentication-manager>
</beans>
```

**intercept-url**

- `pattern` URI 패턴
- `access` 
  - 표현식: 기본값. 권장된다.
  - 문자열: `use-expressions="false"` 

**web.xml**

```xml
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring/root-context.xml
            
             <!-- 추가 -->
			/WEB-INF/spring/security-context.xml
		</param-value>
	</context-param>

<!-- 로그인 -->
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**SampleController.java**

```java
@Log4j
@RequestMapping("/sample/*") 
@Controller
public class SampleController {

  
  @GetMapping("/all")
  public void doAll() {
    
    log.info("do all can access everybody");
  }
  
  @GetMapping("/member")
  public void doMember() {
    
    log.info("logined member");
  }
  
  @GetMapping("/admin")
  public void doAdmin() {
    
    log.info("admin only");
  }  
  
  
  @PreAuthorize("hasAnyRole('ROLE_ADMIN','ROLE_MEMBER')")
  @GetMapping("/annoMember")
  public void doMember2() {
    
    log.info("logined annotation member");
  }
  
  
  @Secured({"ROLE_ADMIN"})
  @GetMapping("/annoAdmin")
  public void doAdmin2() {

    log.info("admin annotaion only");
  }
}
```

views/sample 에 JSP 파일 추가

- admin
- all
- member



- `username` 사용자 ID
- `User` 인증정보와 권한을 가진 객체

**security-context.xml**

```xml
<security:authentication-manager> 
    <security:authentication-provider> 
        <security:user-service> 
            <security:user name="member" password="{noop}member" authorities="ROLE_MEMBER"/> 
            <!-- 				<security:user name="admin" password="{noop}admin" authorities="ROLE_MEMBER, ROLE_ADMIN"/>  -->
        </security:user-service> 
    </security:authentication-provider> 
</security:authentication-manager>
```

`{noop}` PasswordEncoder 를 사용하지 않으려면 붙인다.

로그아웃 방법: 개발자도구 - Application - Cookies - `http://localhost` 우클릭 - clear

**admin.jsp**

```jsp
<%@ taglib uri="http://www.springframework.org/security/tags" prefix="sec" %>

<!-- 바디 -->
<h1>/sample/admin page</h1>
<a href="/customLogout">Logout</a>
```



**[접근제한]**

```xml
<security:http>
    <!-- 접근제한 -->
	<security:access-denied-handler ref="customAccessDenied" />
</security:http>
```

**CommonController.java**

```java
@Controller
@Log4j
public class CommonController {

	@GetMapping("/accessError")
	public void accessDenied(Authentication auth, Model model) {

		log.info("access Denied : " + auth);

		model.addAttribute("msg", "Access Denied");
	}

	@GetMapping("/customLogin")
	public void loginInput(String error, String logout, Model model) {

		log.info("error: " + error);
		log.info("logout: " + logout);

		if (error != null) {
			model.addAttribute("error", "Login Error Check Your Account");
		}

		if (logout != null) {
			model.addAttribute("logout", "Logout!!");
		}
	}

	@GetMapping("/customLogout")
	public void logoutGET() {

		log.info("custom logout");
	}

	@PostMapping("/customLogout")
	public void logoutPost() {

		log.info("post custom logout");
	}
}
```

**accessError.jsp**

```jsp
<h1>Access Denied Page</h1>
<h2><c:out value="${SPRING_SECURITY_403_EXCEPTION.getMessage()}"/></h2>
<h2><c:out value="${msg}"/></h2>
```

p.628 인터페이스 직접구현

**[커스텀 로그인]**

**security-context.xml**

```xml
<security:http>
	<security:form-login login-page="/customLogin"/>
</security:http>
```

**controller**

```java
@GetMapping("/customLogin")
public void loginInput(String error, String logout, Model model) {
    log.info("error: " + error);
    log.info("logout: " + logout);

    if (error != null) {
        model.addAttribute("error", "Login Error Check Your Account");
    }

    if (logout != null) {
        model.addAttribute("logout", "Logout!!");
    }
}
```

**CustomLogin.jsp**

```jsp
<h1>Custom Login Page</h1>
<h2><c:out value="${error}" /></h2>
<h2><c:out value="${logout}" /></h2>

<form method='post' action="/login">
    <div>
        <input type='text' name='username' autofocus>
    </div>
    <div>
        <input type='password' name='password'>
    </div>
    <div>
        <input type='checkbox' name='remember-me'> Remember Me
    </div>

    <div>
        <input type='submit'>
    </div>
    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
</form>
```

**[CSRF]**

p.633: Cross-site request forgery

**[커스텀로그인]**

p. 637

**커스텀로그인 핸들러**

```java
@Log4j
@Component("customLoginSuccess")
public class CustomLoginSuccessHandler implements AuthenticationSuccessHandler {

	@Override
	public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication auth)
			throws IOException, ServletException {
		log.warn("Login Success");
		List<String> roleNames = new ArrayList<>();

		auth.getAuthorities().forEach(authority -> {
			roleNames.add(authority.getAuthority());
		});

		log.warn("ROLE NAMES: " + roleNames);

		if (roleNames.contains("ROLE_ADMIN")) {
			response.sendRedirect("/sample/admin");
			return;
		}

		if (roleNames.contains("ROLE_MEMBER")) {
			response.sendRedirect("/sample/member");
			return;
		}
		response.sendRedirect("/");
	}
}
```

**security-context.xml**

```xml
<!-- 빈 생성 -->
<context:component-scan base-package="io.github.dev_connor" />

<security:http>
    <!-- 추가 -->
    <security:form-login login-page="/customLogin" authentication-success-handler-ref="customLoginSuccess"/>
</security:http>
```



**[커스텀 로그아웃]**

**security-context.xml**

```xml
<security:logout logout-url="/customLogout" invalidate-session="true" />
```

- `delete-cookies="remember-me,JSESSION_ID"` 

**[DB로 로그인]**

```
USERNAME	PASSWORD ENABLED
--------------------------
user00		pw00	1
member00	pw00	1
admin00		pw00	1
```

```
USERNAME	AUTHORITY                                         
--------------------------
admin00		ROLE_ADMIN                                        
admin00		ROLE_MANAGER                                      
member00	ROLE_MANAGER                                      
user00		ROLE_USER       
```



```sql
create table users(
      username varchar2(50) not null primary key,
      password varchar2(50) not null,
      enabled char(1) default '1');

      
 create table authorities (
      username varchar2(50) not null,
      authority varchar2(50) not null,
      constraint fk_authorities_users foreign key(username) references users(username));
      
 create unique index ix_auth_username on authorities (username,authority);


insert into users (username, password) values ('user00','pw00');
insert into users (username, password) values ('member00','pw00');
insert into users (username, password) values ('admin00','pw00');

insert into authorities (username, authority) values ('user00','ROLE_USER');
insert into authorities (username, authority) values ('member00','ROLE_MANAGER'); 
insert into authorities (username, authority) values ('admin00','ROLE_MANAGER'); 
insert into authorities (username, authority) values ('admin00','ROLE_ADMIN');
commit;
```

**커스텀 테이블**

```sql
create table tbl_member(
      userid varchar2(50) not null primary key,
      userpw varchar2(100) not null,
      username varchar2(100) not null,
      regdate date default sysdate, 
      updatedate date default sysdate,
      enabled char(1) default '1');


create table tbl_member_auth (
     userid varchar2(50) not null,
     auth varchar2(50) not null,
     constraint fk_member_auth foreign key(userid) references tbl_member(userid)
);
```



**[DB연결]**

스프링 시큐리티가 기본적으로 이용하는 테이블 구조이다.

**security-context.xml**

```xml
<!-- DB 연결 -->
<security:authentication-manager> 
    <security:authentication-provider>
        <security:jdbc-user-service data-source-ref="dataSource"/>
        <security:password-encoder ref="customPasswordEncoder"/>
    </security:authentication-provider>
</security:authentication-manager>
```

- `dataSource` root-context.xml 의 DB 빈이다.



**패스워드 인코더**

```java
@Log4j
@Component("customPasswordEncoder")
public class CustomNoOpPasswordEncoder implements PasswordEncoder {
	public String encode(CharSequence rawPassword) {
		log.warn("before encode :" + rawPassword);
		return rawPassword.toString();
	}

	public boolean matches(CharSequence rawPassword, String encodedPassword) {
		log.warn("matches: " + rawPassword + ":" + encodedPassword);
		return rawPassword.toString().equals(encodedPassword);
	}
}
```

**[사용자 지정 테이블]**

쿼리문에서 ALIAS 를 피하기 위해 칼럼명을

- username
- password
- enabled
- authority 

로 생성하자.

**jdbc-user-service 속성**

- `users-by-username-query` 
- `authorities-by-user-name-query` 

**예시**

```xml
<jdbc-user-service data-source-ref="dataSource" users-by-username-query=
                   "SELECT name AS userName,password, enabled 
                    FROM user_list 
                    WHERE name=?"
                   authorities-by-username-query=
                   "SELECT name AS userName, authority 
                    FROM user_list 
                    WHERE name=?" />
```

구글링해서 예시를 가져왔다.

**[비밀번호 암호화]**

p.651

**security-context.xml**

```xml
<bean id="bcryptPasswordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"></bean>

	<security:authentication-manager> 
		<security:authentication-provider>
			<security:jdbc-user-service 
				data-source-ref="dataSource"
				users-by-username-query="
					SELECT userid username, userpw password, enabled
					FROM tbl_member
					WHERE userid = ?"
				authorities-by-username-query="
					SELECT userid username, auth authority
					FROM tbl_member_auth
					WHERE userid = ?"
			/>
			<security:password-encoder ref="bcryptPasswordEncoder"/>
		</security:authentication-provider>
	</security:authentication-manager>
```



**멤버생성 테스트**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({
  "file:src/main/webapp/WEB-INF/spring/root-context.xml",
  "file:src/main/webapp/WEB-INF/spring/security-context.xml"
  })
@Log4j
public class MemberTests {

  @Setter(onMethod_ = @Autowired)
  private PasswordEncoder pwencoder;
  
  @Setter(onMethod_ = @Autowired)
  private DataSource ds;
  
  @Test
  public void testInsertMember() {
    String sql = "insert into tbl_member(userid, userpw, username) values (?,?,?)";

    for(int i = 0; i < 100; i++) {
      Connection con = null;
      PreparedStatement pstmt = null;
      
      try {
        con = ds.getConnection();
        pstmt = con.prepareStatement(sql);
        pstmt.setString(2, pwencoder.encode("pw" + i));
        
        if(i <80) {
          pstmt.setString(1, "user"+i);
          pstmt.setString(3,"일반사용자"+i);
        }else if (i <90) {
          pstmt.setString(1, "manager"+i);
          pstmt.setString(3,"운영자"+i);
        }else {
          pstmt.setString(1, "admin"+i);
          pstmt.setString(3,"관리자"+i);
        }
        pstmt.executeUpdate();
      }catch(Exception e) {
        e.printStackTrace();
      }finally {
        if(pstmt != null) { try { pstmt.close();  } catch(Exception e) {} }
        if(con != null) { try { con.close();  } catch(Exception e) {} }
      }
    }//end for
  }
  
  @Test
  public void testInsertAuth() {
    String sql = "insert into tbl_member_auth (userid, auth) values (?,?)";
    
    for(int i = 0; i < 100; i++) {
      Connection con = null;
      PreparedStatement pstmt = null;

      try {
        con = ds.getConnection();
        pstmt = con.prepareStatement(sql);

        if(i <80) {
          pstmt.setString(1, "user"+i);
          pstmt.setString(2,"ROLE_USER");
        }else if (i <90) {
          pstmt.setString(1, "manager"+i);
          pstmt.setString(2,"ROLE_MEMBER");
        }else {
          pstmt.setString(1, "admin"+i);
          pstmt.setString(2,"ROLE_ADMIN");
        }
        pstmt.executeUpdate();
      }catch(Exception e) {
        e.printStackTrace();
      }finally {
        if(pstmt != null) { try { pstmt.close();  } catch(Exception e) {} }
        if(con != null) { try { con.close();  } catch(Exception e) {} }
      }
    }//end for
  }
}
```

> 테스트할 때는 dataSource 를 활용하는 듯 하다.
>
> 패스워드 인코더가 다른파일이 존재한다면 에러가 난다.

```xml
<security:authentication-manager> 
    <security:authentication-provider user-service-ref="customUserDetailsService">
        <security:password-encoder ref="bcryptPasswordEncoder"/>
    </security:authentication-provider>
</security:authentication-manager>
```



**[UserDetailsService]**

**유저 객체**

```java
@Data
public class MemberVO {
	private String userid;
	private String userpw;
	private String userName;
	private boolean enabled;
	private Date regDate;
	private Date updateDate;
	private List<AuthVO> authList;
}
```

**권한 객체**

```java
@Data
public class AuthVO {
  private String userid;
  private String auth;
}
```

**유저 Mapper**

```java
public interface MemberMapper {
	public MemberVO read(String userid);
}
```

**MemberMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="io.github.dev_connor.mapper.MemberMapper">

	<resultMap type="MemberVO" id="memberMap">
		<id property="userid" column="userid" />
		<result property="userid" column="userid" />
		<result property="userpw" column="userpw" />
		<result property="userName" column="username" />
		<result property="regDate" column="regdate" />
		<result property="updateDate" column="updatedate" />
		<collection property="authList" resultMap="authMap">
		</collection>
	</resultMap>

	<resultMap type="AuthVO" id="authMap">
		<result property="userid" column="userid" />
		<result property="auth" column="auth" />
	</resultMap>

	<select id="read" resultMap="memberMap">
		SELECT
		mem.userid, userpw, username, enabled, regdate, updatedate, auth
		FROM
		tbl_member mem LEFT OUTER JOIN tbl_member_auth auth on mem.userid = auth.userid
		WHERE mem.userid = #{userid}
	</select>
</mapper>
```

**root-context.xml**

```xml
<value>io.github.dev_connor.domain.MemberVO</value>
<value>io.github.dev_connor.domain.AuthVO</value>
```

ALIAS 를 등록한다.

**더미 데이터**

```sql
INSERT INTO tbl_member (userid, userpw, username)
VALUES ('admin90', '$2a$10$WMZ', '관리자90');
INSERT INTO tbl_member_auth 
VALUES ('admin90', 'ROLE_ADMIN');COMMIT;
```



**Mapper 테스트**

```
|--------|-----------|---------|---------|----------------------|----------------------|-----------|
|userid  |userpw     |username |enabled  |regdate               |updatedate            |auth       |
|--------|-----------|---------|---------|----------------------|----------------------|-----------|
|admin90 |$2a$10$WMZ |관리자90    |[unread] |2022-02-12 00:54:31.0 |2022-02-12 00:54:31.0 |ROLE_ADMIN |
|--------|-----------|---------|---------|----------------------|----------------------|-----------|
```



```java
@RunWith(SpringRunner.class)
@ContextConfiguration({"file:src/main/webapp/WEB-INF/spring/root-context.xml"})
@Log4j
public class MemberMapperTests {

  @Setter(onMethod_ = @Autowired)
  private MemberMapper mapper;
  
  @Test
  public void testRead() {
    MemberVO vo = mapper.read("admin90");
    log.info(vo);
    vo.getAuthList().forEach(authVO -> log.info(authVO));
  }
}
```

**로그인 유저 객체**

```java
@Getter
public class CustomUser extends User {
	private static final long serialVersionUID = 1L;
	private MemberVO member;

	public CustomUser(String username, String password, 
			Collection<? extends GrantedAuthority> authorities) {
		super(username, password, authorities);
	}

	public CustomUser(MemberVO vo) {
		super(vo.getUserid(), vo.getUserpw(), vo.getAuthList().stream()
				.map(auth -> new SimpleGrantedAuthority(auth.getAuth())).collect(Collectors.toList()));
		this.member = vo;
	}
}
```



**유저 서비스**

```
WARN : io.github.dev_connor.security.CustomUserDetailsService - Load User By UserName : admin90
WARN : io.github.dev_connor.security.CustomUserDetailsService - queried by member mapper: MemberVO(userid=admin90, userpw=$2a$10$WMZ, userName=관리자90, enabled=false, regDate=Sat Feb 12 00:54:31 KST 2022, updateDate=Sat Feb 12 00:54:31 KST 2022, authList=[AuthVO(userid=admin90, auth=ROLE_ADMIN)])
WARN : io.github.dev_connor.security.CustomNoOpPasswordEncoder - matches: $2a$10$WMZ:$2a$10$WMZ
WARN : io.github.dev_connor.security.CustomLoginSuccessHandler - Login Success
WARN : io.github.dev_connor.security.CustomLoginSuccessHandler - ROLE NAMES: [ROLE_ADMIN]
```



**security-context.xml**

```xml
<!-- DB 연결 -->
<security:authentication-manager> 
    <security:authentication-provider user-service-ref="customUserDetailsService">
        <security:password-encoder ref="customPasswordEncoder"/>
    </security:authentication-provider>
</security:authentication-manager>
```



```java
@Log4j
@Service
public class CustomUserDetailsService implements UserDetailsService {

	@Setter(onMethod_ = { @Autowired })
	private MemberMapper memberMapper;

	@Override
	public UserDetails loadUserByUsername(String userName) throws UsernameNotFoundException {
		log.warn("Load User By UserName : " + userName);
		MemberVO vo = memberMapper.read(userName);
		log.warn("queried by member mapper: " + vo);
		return vo == null ? null : new CustomUser(vo);
	} 
}
```

**[JSP]**

```jsp
<%@ taglib uri="http://www.springframework.org/security/tags" prefix="sec" %>

	<!-- 바디 -->
	<p>principal : <sec:authentication property="principal"/></p>
	<p>MemberVO : <sec:authentication property="principal.member"/></p>
	<p>사용자이름 : <sec:authentication property="principal.member.userName"/></p>
	<p>사용자아이디 : <sec:authentication property="principal.username"/></p>
	<p>사용자 권한 리스트  : <sec:authentication property="principal.member.authList"/></p>
```





**sec:authorize**

- `isAnonymous()` 
- `isAuthenticated()` 



**로그인 성공 후 페이지**

```java
@Log4j
@Component("customLoginSuccess")
public class CustomLoginSuccessHandler implements AuthenticationSuccessHandler {

	@Override
	public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication auth)
			throws IOException, ServletException {
		log.warn("Login Success");
		List<String> roleNames = new ArrayList<>();

		auth.getAuthorities().forEach(authority -> {
			roleNames.add(authority.getAuthority());
		});
		log.warn("ROLE NAMES: " + roleNames);
		response.sendRedirect("/board/list");
	}
}
```



**더미**

```java
if (roleNames.contains("ROLE_ADMIN")) {
    response.sendRedirect("/sample/admin");
    return;
}

if (roleNames.contains("ROLE_MEMBER")) {
    response.sendRedirect("/sample/member");
    return;
}
```

**[자동 로그인]**

p.676

**[권한으로 페이지이동]**



p. 701

- `@Secured(문자열 or 문자열배열)`
- `@PreAuthorize(표현식)` 3버전부터 지원.
- `@PostAuthorize(표현식)` 3버전부터 지원.



**표현식 **

- `hasRole()` `hasAuthority()`
- `hasAnyRole('ROLE_ADMIN','ROLE_MEMBER')` `hasAnyAuthority()` 
- `principal` 
- `permitAll`
- `denyAll`
- `isAnonymous()`
- `isAuthenticated()`
- `isFullyAuthenticated()`



**sevlet-context.xml**

```
http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security-5.0.xsd
```

위를 아래와 같이 `-5.0` 을 지운다.

```
xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
```

그 후 아래코드를 추가한다.

```xml
<security:global-method-security pre-post-annotations="enabled" secured-annotations="enabled"/>
```



**글 작성: BoardController.java**

```java
@GetMapping("/register")
@PreAuthorize("isAuthenticated()")
public void register() {}

@PostMapping("/register")
@PreAuthorize("isAuthenticated()")
public String register(BoardVO board, RedirectAttributes rttr) {
```



**[로그인 후 글작성 페이지]**

```xml
<security:form-login login-page="/customLogin"/> 
```

`authentication-success-handler-ref` 속성을 없앤다.

**security-context.xml**

```xml
<security:form-login login-page="/customLogin" default-target-url="/board/list"/>
```

`default-target-url` 이 속성에 url 를 준다.

**[게시글 수정삭제 권한]**

**get.jsp**

```jsp
<sec:authentication property="principal" var="p"/>
<sec:authorize access="isAuthenticated()">
    <c:if test="${p.username eq board.writer }">
        <button data-oper='modify' class="btn btn-default">
            <a href="/board/modify?bno=<c:out value='${board.bno}'/>">수정</a>
        </button>			
        <form role="form" action="/board/remove?bno=${board.bno }" method="post">
            <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
            <button type="submit" data-oper='remove' class="btn btn-danger">삭제</button>
        </form>
    </c:if>
</sec:authorize>
```

작성자만 수정/삭제할 수 있다.

## 회원가입

**컨트롤러**

```java
@Controller
@Log4j
@AllArgsConstructor
public class MemberController {
	private static final Logger logger = LoggerFactory.getLogger(BoardController.class);
	private MemberService service;

	@GetMapping("/join")
	public void joinGET() {
		log.info("get join");
	}
	
	@PostMapping("/join")
	public String joinPost(MemberVO member) {
		log.info("post join: " + member);
		service.join(member);
		return "redirect:/board/list";
	}
}
```

**서비스**

```java
@Log4j
@Service
@AllArgsConstructor
public class MemberServiceImpl implements MemberService {

	@Setter(onMethod_ = @Autowired)
	private MemberMapper mapper;
	
	@Setter(onMethod_ = @Autowired)
	private PasswordEncoder pwencoder;

	@Override
	public void join(MemberVO member) {
		String encodedPW = pwencoder.encode(member.getUserpw());
		member.setUserpw(encodedPW);
		mapper.join(member);
	}
}
```

**MyBatis**

```xml
<insert id="join">
    BEGIN
        INSERT INTO tbl_member (userid, userpw, username)
        VALUES (#{userid}, #{userpw}, #{userName});

        INSERT INTO tbl_member_auth
        VALUES (#{userid}, 'ROLE_USER');
    END;
</insert>
```

오라클에서 다중쿼리를 사용하려면 PL/SQL 을 사용하면 된다.













