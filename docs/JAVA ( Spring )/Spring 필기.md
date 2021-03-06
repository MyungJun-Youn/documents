# 01. 스프링이란?
---

#### 프레임워크
	특정한 목적에 맞게 프로그래밍을 쉽게 하기 위한 약속

#### 스프링(SPRING)
	자바언어를 기반으로, 다양한 어플리케이션을 제작하기 위한 약속된 프로그래밍 틀

	예전 EJB의 경우 고가의 장비가 필요 되어지고, 개발환경 및 설정 그리고 테스트 환경에 많은 애로사항들이 존재했다.
	하지만 스프링의 경우 톰캣을 이용할 수 있다. EJB에 비해서 코드의 경량화 그리고 개발 중에 테스트가 쉽다는 점이 특징

	국내 자바개발자들에게 표준프레임워크

# 02. 스프링 프로젝트 만들기
---

#### DI(Dependency Injection)와 IOC컨테이너
1. DI(Dependency Injection)
	- 방법1(내가 직접 객체를 생성한다.), 방법2(객체를 외부에 생성하여 넣어준다. 주입)
	- 방법2가 더 좋다. (스프링 사용)
2. IOC컨테이너
	- 객체를 부품화 시키고, 그 부품들의 집합

# 03. DI(Dependency Injection) - I
---

# 04. DI(Dependency Injection) - II
---

# 05. DI 활용
---

 - 규모가 커지고, 추후 유지보수 업무가 발생할 경우 DI를 이용한 개발의 장점이 될 수 있다.
 - **자바파일의 수정 없이 스프링 설정 파일만을 수저앟여 부픔들을 생성/조립할 수 있다.**

# 06. DI설정 방법
---

 - <bean>내부에서 'c:'는 constructor-arg의 약자, 'p:'는 property의 약자
 - 위의 약자를 사용 하려면

	xmlns:c="http://www.springframework.org/schema/c"<br/>
	xmlns:p="http://www.springframework.org/schema/p"
	를 추가해 줘야 한다.

#### JAVA로 DI설정
 - @Configuration<br/>
   이 클래스는 스프링 설정에 사용되는 클래스입니다. 라고 명시해 주는 어노테이션. 꼭 작성해야 한다.
 - @Bean<br/>
   객체 생성
 - 이렇게 만들어진 java파일의 class는 'AnnotationConfigApplicationContext'를 사용하여 호출하여 사용한다.<br/>
   AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(\<ClassName>.class);

#### xml안에 JAVA로 DI설정
 - <context:annotation-config /> <br/>
   을 선언하고, 그 아래에 \<bean class="\<className>" />으로 config파일을 불러올 수 있다.

#### JAVA안에 xml로 DI설정
 - @ImportResource("classpath:\<xml path>")<br/>
   JAVA파일안에 위와 같이 생성해 놓은 xml파일을 불러와 사용할 수 있다.

# 07. 생명주기와 범위
---

#### 스프링 컨테이너 생명주기
	GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();	// 생성

	ctx.load("classpath:applicationCTX.xml");	// 설정

	ctx.refresh();
	// .load를 할 경우 .refresh()를 호출해줘야 한다.

	Student student = ctx.getBean("student", Student.class);	// 사용
	System.out.println("이름 : " + student.getName());
	System.out.println("나이 : " + student.getAge());

	ctx.close();															// 종료

#### 스프링 빈 생명주기
 - 컨테이너에서 .refresh()를 호출 할 경우 bean이 생성된다.
 - 컨테이너가 .close()로 소멸할 경우, bean도 같이 소멸하게 된다.
 - bean만 소멸시키고 싶을 경우 beand에서 .destroy()를 호출하여 소멸시킨다.
 - bean이 생성하고 소멸할 때에 실행되는 메소드를 만들고 싶을 경우<br/>
   @PostConstruct, @PreDestroy<br/>
   각 어노테이션을 생성, 소멸대 실행될 메소드위에 적어준다.

#### 스프링의 범위
 - 생성된 스프링 빈은 scope을 가지고 있습니다.
 - 기본적으로 명시하지 않으면 scope="singleton"으로 설정이 된다.

# 08. 외부파일을 이용한 설정
---

#### Environment 객체
	ConfigurableApplicationContext ctx = new GenericXmlApplicationContext();
	ConfigurableEnvironment env = ctx.getEnvironment();
	// ctx의 Environmnet를 읽어 온다.
	MutablePropertySources propert	ySources = env.getPropertySources();
	// env의 property들을 가져온다.
	propertySources.addLast(new ResourcePropertySource("classpath:admin.properties"));
	// property를 추가해 준다.

	setAdminId(env.getProperty("admin.id"));
	setAdminPw(env.getProperty("admin.pw"));
	// 추출하여 사용한다.

#### xml파일에 프로퍼티 파일을 이용한 설정
	<context:property-placeholder location="classpath:admin.properties, classpath:sub_admin.properties" />
	// xml파일에 위와 같이 properties파일을 읽어와 사용한다.
 - ${ 변수명 }을 사용하여 값을 넣어줄 수 있다. (properties에 있는 변수명)

#### JAVA파일에 프로퍼티 파일을 이용한 설정
	@Configuration
	public class ApplicationConfig {

	// propertices의 있는 내용을 바로 적용해준다.
	@Value("${admin.id}")
	private String adminId;
	@Value("${admin.pw}")
	private String adminPw;
	@Value("${sub_admin.id}")
	private String sub_adminId;
	@Value("${sub_admin.pw}")
	private String sub_adminPw;

	@Bean
	public static PropertySourcesPlaceholderConfigurer Properties() {
	// 반드시 작성해야 하는 메소드. propertices를 설정한다.
		PropertySourcesPlaceholderConfigurer configurer = new PropertySourcesPlaceholderConfigurer();

		Resource[] locations = new Resource[2];
		locations[0] = new ClassPathResource("admin.properties");
		locations[1] = new ClassPathResource("sub_admin.properties");
		configurer.setLocations(locations);

		return configurer;
	}

	@Bean
	public AdminConnection adminConfig() {
		AdminConnection adminConnection = new AdminConnection();
		adminConnection.setAdminId(adminId);
		adminConnection.setAdminPw(adminPw);
		adminConnection.setSub_adminId(sub_adminId);
		adminConnection.setSub_adminPw(sub_adminPw);
		return adminConnection;
	}

}

#### xml파일에 profile속성을 사용한 설정
	profile="dev"

	GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
	ctx.getEnvironment().setActiveProfiles(config);
	ctx.load("applicationCTX_dev.xml", "applicationCTX_run.xml");

- beans의 속성으로 위와 같이 profile의 이름을 설정해 준다.
- java파일에서 아래와 같이 불러오면 profile의 이름이 config에 해당하는 파일을 읽어온다.



#### JAVA파일에 profile속성을 사용한 설정
	@Profile("dev")

	AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
	ctx.getEnvironment().setActiveProfiles(config);
	ctx.register(ApplicationConfigDev.class, ApplicationConfigRun.class);
	ctx.refresh();

 - profile에 해당하는 .class파일을 읽어온다.

# 09. AOP (Aspect Oriented Programming) - I
---
#### AOP란?
 - 공동 기능을 사용하는 경우 모든 모듈에 적용하기 위한 방법
 - 상속의 경우는 다중 상속이 안 될 경우 공통기능 부여에 한계가 있다. 그리고 기능 구현부분에 핵심 기능 코드와 공통 기능 코드가 섞여 있어 효율성이 떨어진다.
 - AOP방법은 핵심 기능과 공통 기능을 분리 시켜놓고, 공통 기능을 필요로 하는 핵심 기능들에서 사용하는 방식입니다.

		Aspect : 공통 기능
		Advice : Aspect의 기능 자체
		Jointpoint : Advice를 적용해야 되는 부분( ex, 필드, 메소드 ) (스프링에서는 메소드만 해당)
		Pointcut : Jointpoint의 부분으로 실제로 Advice가 적용된 부분, Jointpoint의 세분화
		Weaving : Advice를 핵심 기능에 적용 하는 행위

#### XML기반의 AOP구현
**의존성 설정**

	<!-- AOP : porm.xml에 작성 -->
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjweaver</artifactId>
		<version>1.7.4</version>
	</dependency>

**XML 파일 설정**

	<bean id="logAop" class="com.javalec.ex.LogAop" />

	<aop:config>
		<aop:aspect id="logger" ref="logAop">
			<aop:pointcut id="publicM" expression="within(com.javalec.ex.*)"  />
			<aop:around pointcut-ref="publicM" method="loggerAop" />
		</aop:aspect>
	</aop:config>

**공통 기능 클래스**

	public class LogAop {

		public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable {
			String signatureStr = joinpoint.getSignature().toShortString();
			System.out.println( signatureStr + " is start.");
			long st = System.currentTimeMillis();

			try {
				Object obj = joinpoint.proceed();
				// 핵심 기능 실행
				return obj;
			} finally {
				long et = System.currentTimeMillis();
				System.out.println( signatureStr + " is finished.");
				System.out.println( signatureStr + " 경과시간 : " + (et - st));
			}

		}

	}

#### Advice의 종류
	<aop:before> : 메소드 실행 전에 advice실행 (2번째로 많이 사용)
	<aop:after-returning> : 정상적으로 메소드 실행 후에 advice실행
	<aop:after-throwing> : 메소드 실행중 exception 발생시 advice실행
	<aop:after> : 메소드 실행중 exception 이 발생하여도 advice실행
	<aop:around> : 메서드 실행 전/후 및 exception 발생시 advice실행 (가장 많이 사용)

# 10. AOP (Aspect Oriented Programming) - II
---


# 11. 스프링 MVC 기초
---

#### 스프링 MVC 개요
	client가 요청을 하게 되면 DispatcherServlet이 모든 모듈을 통제하여 client에게 정보를 제공한다.
	Controller를 거쳐 View를 사용자에게 보여준다.

#### 스프링 MVC 구조
`HomeController.java` : Controller의 내용을 작성하는 부분<br/>
`/views` : View 내용을 작성하는 부분<br/>
`servlet-context.xml` : View를 연결해 주는 부분<br/>
`web.xml` : DispatcherServlet서블릿 맵핑, 스프링 설정 파일 위치 정의 <br/>

# 12. 컨트롤러
---
#### 요청 처리 메소드 제작
`@RequestMapping("/board/view")` : 요청 경로(path)<br/>
`return "board/view";` : 뷰페이지 이름

#### 뷰에 데이터 전달
	@RequestMapping("/board/conent")
	public String content(Model model) {		// Model 객체를 파라미터로 받음
		model.addAttribute("id", 30);			// Model 객체에 데이터를 담음
		return "boadr/content";
	}

	@RequestMapping("/board/reply")
	public ModelAndView reply() {
		ModelAndView mv = new ModelAndView();	// ModelAndView 객체 생성
		mv.addObject("id", 30);					// Model 객체에 데이터를 담음
		mv.setViewName("/board/reply");			// 뷰이름 설정

		return mv;
	}

# 13. Form 데이터
---
#### HttpServletRequest
	@RequestMapping("boar/confirmId")
	public String confirmId(HttpServletRequest httpServletRequest, Model model) {
		String id = httpServletRequest.getParameter("id");
		String pw = httpServletRequest.getParameter("pw");
		model.addAttribute("id", id);
		model.addAttribute("pw", pw);
		retrun "board/confirmId";
	}
값이 없어도 Error를 출력하지 않고 값을 출력하지 않고 화면을 보여준다.

#### @RequestParam
	@RequestMapping("board/checkId")
	public String checkId(@RequestParam("id")String id, @RequestParam("pw")int pw, Model model) {
		model.addAttribute("identify", id);
		model.addAttribute("password", ow);
		return "board/checkId";
	}
값이 없을 경우, 값 형식이 다를 경우 400Error를 출력한다.

#### 데이터(커맨드) 객체
	@RequestMapping("/member/join")
	public String joinData(Member member) {
		return "member/join";
	}
값이 없어도 Error가 나지 않는다.
단, 파라미터의 이름은 객체의 내부의 변수 명과 이름이 일치해야한다.

#### @PathVariable
	@RequestMapping("/student/{studentId}")
	public String getStudent(@PathVariable String studentId, Model model) {
		model.addAttribute("studentId", studentId);
		return "student/studentView";
	}

# 14. @RequestMapping 파라미터
---
#### @RequestMapping에서 Get방식과 Post방식

**@RequestMapping에서 요청을 받을 때 Get방식과 Post방식으로 구분 할 수 있습니다.**

	<form action "student" method="get">
		student id : <input type="text" name="id"> <br />
		<input type="submit" value="전송"/>
	</form>


	@RequestMapping(method=RequestMethod.GET, value ="/student")
	public String goStudent(HttpServletRequest httpServletRequest, Model model) {
		Syetem.out.println("RequestMethod.GET");

		String id = httpServletRequest.getParameter("id");
		System.out.println("id : " + id);
		model.addAttribute("studentId", id);

		return "student/studentId";
	}

#### @ModelAttribute
**@ModelAttribute 어노이테이션을 이용하면 커맨드 객체의 이름을 개발자가 변경 할 수 있습니다.**

	@RequestMapping("/studentView")
	public String studentView(StudentInformation studentInformation) {
		return "studentView";
	}

	@RequestMapping("/studentView")
	public String studentView(@ModelAttribute("studentInfo") StudentInformation studentInformation) {
		return "studentView"
	}

#### 리다이렉트(redirect: 키워드)
**다른 페이지로 이동할 때 사용합니다.**

	@RequestMapping("/studentConfirm")
	public String studentRedirect(HttpServletRequest httpServletRequest, Model model) {
		String id = httpServletRequest.getParameter("id");

		if ( id.equals("abc") ) {
			return "redirect:studentOk";
		}

		return "redirect:studentNg";
	}

## *한글 처리*
**web.xml에 다음과 같은 코드를 넣는다.**

	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>
			org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
        	<param-name>forceEncoding</param-name>  
        	<param-value>true</param-value>
    	</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

# 폼 데이터 값 검증
---
client에서 값을 검사하는 것(Javascript, JQuery..)이 아니라 server에서 값을 검사하는 것  

#### Validator를 이용한 검증
폼에서 전달 되는 데이터를 커맨드 객체에 담아서 전달을 한다. 이때 커맨드 객체의 유효성 검사를 할 수 있다.

	@RequestMapping("/student/create")
	public String studentCreate(@ModelAttribute("student") Student student, BindingResult result) {

		String page = "createDonePage";

		StudentValidator validator = new StudentValidator();
		validator.validate(student, result);
		if(result.hasErrors()) {
			page = "createPage";
		}

		return page;
	}


	@Override
	public boolean supports(Class<?> arg0) {
		return Student.class.isAssignableFrom(arg0);  // 검증할 객체의 클래스 타입 정보
	}

	@Override
	public void validate(Object obj, Errors errors) {
		System.out.println("validate()");
		Student student = (Student)obj;

		String studentName = student.getName();
		if(studentName == null || studentName.trim().isEmpty()) {
			System.out.println("studentName is null or empty");
			errors.rejectValue("name", "trouble");
		}

		int studentId = student.getId();
		if(studentId == 0) {
			System.out.println("studentId is 0");
			errors.rejectValue("id", "trouble");
		}
	}

# 15 ~ 20 게시판 만들기
---
**MySQL 설정**

	// porm.xml
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.6</version>
	</dependency>

	// Servers/context.xml
	<Resource
    	name="jdbc/[DBName]"
    	auth="Container"
    	type="javax.sql.DataSource"
		maxActive="100"
		maxIdle="30"
		maxWait="10000"
		username="root"
		password="root"
		driverClassName="com.mysql.jdbc.Driver"
		url="jdbc:mysql://localhost:3306/[DBName]"
	/>

	// web.xml
	<resource-ref>
		<description>DB Connection</description>
		<res-ref-name>jdbc/[DBName]</res-ref-name>
		<res-type>javax.sql.DataSource</res-type>
		<res-auth>Container</res-auth>
	</resource-ref>

**DB연결**

	Context context = new InitialContext();
	DataSource dataSource = (DataSource) context.lookup("java:comp/env/jdbc/[DBName]");
	Connection con = dataSource.getConnection();
	PreparedStatement ps = con.prepareStatement(query);
	ResultSet rs = ps.executeQuery();
	// ps.executeUpdate();

# 21. 스프링 JDBC
---

**servlet-context.xml**

	<beans:bean name="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource'>
		<beans:property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<beans:property name="url" value="jdbc:mysql://localhost:3306/[DBName]" />
		<beans:property name="username" value="root" />
		<beans:property name="password" value="root" />
	</beans:bean>

	<beans:bean name="template" class="org.springframework.jdbc.core.JdbcTemplate">
		<beans:property name="dataSource" ref="dataSource" />
	</beans:bean>

**Controller.java**

	public JdbcTemplate template;

	@Autowired
	public void setTemplate(JdbcTemplate template) {
		this.template = template;
		Constant.template = this.template;
	}

**Constant.java**

	public class Constant {
		public static JdbcTemplate template;
	}

**DAO.java**

	JdbcTemplate template = Constant.template;

**Select**

	template.query(query, [저장형식]);
	// template.query(query, new BeanPropertyRowMapper<BDto>(BDto.class));

	template.queryForObject(query, [저장형식]);

**Insert**

	this.template.update(new PreparedStatementCreator() {
		@Override
		public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
			String query = "";
			PrepareStatement pstmt = con.prepareStatement(query);
			pstmt.setString(#, [name]);

			return pstmt;
		}
	});

	String query = "";
	this.template.update(query, new PreparedStatementSetter() {
		@Override
		public void setValues(PreparedStatement ps) throws SQLException {
			ps.setString(#, [name]);
		}
	})

**Update**

	String query = "";
	this.template.update(query, new PrepareStatementSetter() {
		@Override
		public void setValues(PreparedStatement ps) throws SQLException {
			ps.setString(#, [name]);
		}
	});

**Delete**

	String query = "";
	this.template.update(query, new PrepareStatementSetter() {
		@Override
		public void setValues(PreparedStatement ps) throws SQLException {
			ps.setString(#, [name]);
		}
	})


	출처 : 자바-JSP-Spring 강좌(https://www.youtube.com/playlist?list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9) 요약
