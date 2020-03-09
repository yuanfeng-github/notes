### MVC设计模式 ###
	全称 Model View Controller
	将web项目分为三层架构，展示层，控制层，持久层
	MVC是将一个程序进行了分工，是程序设计的思想
	
	模型：完成具体的业务操作，如:查询数据库，封装对象，javaBean
	视图：展示数据，如:jsp
	控制器：接收请求，处理请求，处理请求并响应结果；
		流程
		1. 获取请求
		2. 调用模型
		3. 将数据交给视图进行展示；
	优点：
			1. 耦合低，方便维护，利于分工协作
			2. 复用性高，

	缺点：
			1. 项目架构变的复杂，对开发人员要求更高


###SpringMVC
	SpringMVC 是一种基于java 采用MVC设计模型的web框架。属于SpringFrameWork的后续产品。它通过一套注解，
	让一个简单的java类成为处理请求的控制类，而无需实现任何借口，同时它还支持RESTful编程风格。
	
	SpringMVC	属于表现层，
	Spring		属于业务层
	
	表现层：	springMVC
	业务层：	spring
	持久层： mybatis

	
###SpringMVC的优势
	1. 清晰的角色划分
		1. 前端控制器(DispatcherServlet)
		2. 请求到处理映射器(HandlerMapping)
		3. 处理适配器(HandlerAdapter)
		4. 视图解析器(ViewResolver)
		5. 页面控制器(Controller)
		6. 验证器(Validator)
		7. 命令对象(Command 请求参数绑定到对象)
		8. 表单对象(Form Object 接收提交上来的表单)
	2. 由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。
	3. 和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。
	4. 可适配，通过 HandlerAdapter 可以支持任意的类作为处理器。
	5. 可定制性，HandlerMapping、ViewResolver 等能够非常简单的定制。
	6. 功能强大的数据验证、格式化、绑定机制。
	7. 利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试。
	8. 本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。
	9. 强大的 JSP 标签库，使 JSP 编写更容易。
	* 还有比如RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配
	置支持等等
	* SpringMVC的框架基于组件方式执行流程


##SpringMVC 和 Struts2 的优略分析
	共同点：
		它们都是表现层框架，都是基于 MVC 模型编写的。
		它们的底层都离不开原始 ServletAPI。
		它们处理请求的机制都是一个核心控制器。
	区别：
		1. Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter
		2.Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类。所以 Spring MVC 会稍微比 Struts2 快些。
		3. Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便(JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。)
		3. Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提升，尤其是 struts2 的表单标签，远没有 html 执行效率高。

###SpringMVC入门

	1. 创建一个maven web项目工程,
	2. 在pom.xml中导入相关依赖

		<!-- 版本锁定 -->
		<properties>
		<spring.version>5.0.2.RELEASE</spring.version>
		</properties>
		<!-- springMVC相关依赖  -->
		<dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

	3. 配置核心前端控制器置DispatcherServlet，web.xml

				<!DOCTYPE web-app PUBLIC
		 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
		 "http://java.sun.com/dtd/web-app_2_3.dtd" >
		
		<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		         xmlns="http://java.sun.com/xml/ns/javaee"
		         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
		         id="WebApp_ID" version="3.0">
		  <display-name>Archetype Created Web Application</display-name>
		
		  <!-- springMVC系统解决POST方式提交中文乱码问题 -->
		  <filter>
		    <filter-name>encodingFiter</filter-name>
		    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		    <init-param>
		      <param-name>encoding</param-name>
		      <param-value>UTF-8</param-value>
		    </init-param>
		  </filter>
		
		  <filter-mapping>
		    <filter-name>encodingFiter</filter-name>
		    <url-pattern>/*</url-pattern>
		  </filter-mapping>
		  <!-- SpringMVC的核心控制器 -->
		  <servlet>
		    <servlet-name>dispatcherServlet</servlet-name>
		    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		    <!-- 配置Servlet的初始化参数，读取springmvc的配置文件，创建spring容器 -->
		    <init-param>
		      <param-name>contextConfigLocation</param-name>
		      <param-value>classpath:springmvc.xml</param-value>
		    </init-param>
		    <!-- 配置servlet启动时加载对象 -->
		    <load-on-startup>1</load-on-startup>
		  </servlet>
		  <servlet-mapping>
			<!-- 拦截路径 -->
		    <servlet-name>dispatcherServlet</servlet-name>
		    <url-pattern>/</url-pattern>
		  </servlet-mapping>
		</web-app>


	4. resources中创建文件	springmvc.xml
					
					<?xml version="1.0" encoding="UTF-8"?>
			<beans xmlns="http://www.springframework.org/schema/beans"
			       xmlns:mvc="http://www.springframework.org/schema/mvc"
			       xmlns:context="http://www.springframework.org/schema/context"
			       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			       xsi:schemaLocation="
			http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/mvc
			http://www.springframework.org/schema/mvc/spring-mvc.xsd
			http://www.springframework.org/schema/context
			http://www.springframework.org/schema/context/spring-context.xsd">
			    
				<!-- 配置spring创建容器时要扫描的包 -->
			    <context:component-scan base-package="cn.yf.springmvc"></context:component-scan>

			    <!-- 配置视图解析器 -->
			    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			        <property name="prefix" value="/WEB-INF/pages/"></property>
			        <property name="suffix" value=".jsp"></property>
			    </bean>

			    <!-- 配置spring开启注解mvc的支持-->
			    <mvc:annotation-driven />

			</beans>
	
		5. WEB-INF下新建pages文件夹，新建hello.jsp页面，
				<%@ page contentType="text/html;charset=UTF-8" language="java" %>
				<html>
				<head>
				    <title>Title</title>
				</head>
				<body>
				<h3>hello SpringMVC</h3>
				</body>
				</html>
		6. 创建相应包结构，
			package cn.yf.springmvc;
			import org.springframework.stereotype.Controller;
			import org.springframework.web.bind.annotation.RequestMapping;
			@Controller
			public class HelloController {
			
			    @RequestMapping("/hello")
			    public String sayHello(){
			        System.out.println("hello SpringMVC中文");
			        return "hello";
			    }
			
			}

		7. 项目部署到tomcat上，
		8. 浏览器访问	http://localhost:8080/hello

### 执行流程 ###
	1. 启动服务器
		1. DispatcherServlet对象被创建  
			 `<load-on-startup>1</load-on-startup>`
		2. springmvc.xml被加载		
			`<param-value>classpath:springmvc.xml</param-value>`
		3. spring容器管理的创建对象（被相关注解的类）	
			` <context:component-scan base-package="cn.yf.springmvc" />`
			@Component
				类之前，单元组件，spring将扫描到当前类，并添加为管理对象
			@Repository
				类之前，dao，数据库访问,Spring中和Component 作用一致，语义不同
			@service
				类之前，业务，Spring中和Component 作用一致，语义不同
			@Controller
				类之前，控制器，Sevlet，Spring中和Component 作用一致，语义不同

	2. 请求处理流程
		1. 发生请求到服务器，DispatcherServlet根据请求的url找到相应的Controller,Controller处理后将数据返回给DispatcherServlet，DispatcherServlet再将数据交个InternalResourceViewResolver 视图解析器，解析器将处理好的ModelAndView返回给DispatcherServlet，DispatcherServlet在将处理好的响应给客户端；
###注解说明
	@RequestMapping(参数)		请求路径
		value	 
		path 与value作用相同
		method					请求方式
		params					指定请求参数条件
		headers					请求中必须包含请求头
	
		@RequestMapping(path = "/login",params = {"username","password"},
            method = {RequestMethod.POST,RequestMethod.GET} )

	method	 请求方式是一个枚举类型常用 get、post method="get"
    params	 指定限定请求参数 params={"username","id=123"}//请求参数必须包含username,并且id=123
	heads	 必须包含Sccept请求头 heads="Sccept"

###请求参数的绑定

	1. 封装对象
		接收参数为对象时，springMVC自动会封装，前提是一个标准的javabean，
		1. 对象中的属性是一个对象类型时
			1. 前端通过表达式 属性名.属性
			<input type="text" name="user.name">
		2. 对象中的属性是list时 ，
			1. list中是对象 ，属性[索引].属性
				<td><input type="text" name="list[0].name"></td>
			2. list ，属性名[]
				<td><input type="text" name="list[0]"></td>
		3.  对象中的属性是map时 属性名[key]
				<td><input type="text" name="list[`key`].name"></td>
	2. 自定义类型转换器		
	请求参数都是String类型，Spring提供了很多转换器，标准的bean对象，框架会调用与参数和对象相应的getset方法进行赋值
	使用了框架中无法识别的类型时，如date 2000/11/11 框架可以解析为date类型，而使用2000-11-11则会发生转换异常，
		解决方法
		需要定义一个类型转换器类
		1. 类需要实现Converter接口，Converter<接收的类型,需要转换的类型> //导入包 import org.springframework.core.convert.converter.Converter;
		2.  定义类实现方法
			public class StringToDateConver implements Converter<String,Date> {
			    @Override
			    public Date convert(String s) {
			        if(s!=null && s.length()>0){
			            DateFormat format=new SimpleDateFormat("yyyy-MM-dd");
			            try {
			                return format.parse(s);
			            } catch (ParseException e) {
			                e.printStackTrace();
			            }
			        }else {
			            throw new RuntimeException("参数为空");
			        }
			        return null;
			    }
			}

		3.  前端控制器配置，注册类型转换器  springmvc.xml
			1.  conversion-service="conversionService2" 注册到converters集合中
			<!--注册类型转换换器-->
		    <bean id="conversionService2" class="org.springframework.context.support.ConversionServiceFactoryBean">
		        <property name="converters">
		            <set>
		                <bean class="cn.yf.utils.StringToDateConver"></bean>
		            </set>
		        </property>
		    </bean>
		    <!-- 配置spring开启注解mvc的支持-->
		    <mvc:annotation-driven conversion-service="conversionService2"/>


### 常用注解 ###
	
	@RequestParam 			//作用于方法参数，设置请求参数名称，默认请求参数必须，
		属性
		value	名称
		name	与value一致
		required，默认为true,true表示必须传入该参数，false表示不传入
	
	@RequestBody			//作用于方法参数，获取体内容为key=value&key=value 结构的数据,get请求不适用
		属性
			value	名称
			name	与value一致
			required:是否必须有请求体，默认为true,为true时，get请求会报错，如果为false，get请求得到为null

	@PathVaribale		//作用于方法参数，用于绑定url中的占位符， /delete/{id} id就是url占位符 spring3.0之后加入
		属性
		value  指定url占位名称，
		required	是否为必须

	@ModelAttribute			//spring 4.3之后才有的，作用于方法和参数，作用，表示当前方法会在控制器方法之前执行，

	1. 修饰拥有返回值的方法，请求方法获取的参数是该方法返回的数据，
		@ModelAttribute
		public User initUser(String name){
			User user=new User();
			user.setName(name);
			//参数时会使用参的值，没有则使用这个值
			user.setAge(20);
			return user;
		}

	2. 修饰没有返回值的方法使用
		@ModelAttribute
		public User initUser(String username,Map<String,User> map){
			User user=new User();
			user.setName(username);
			//参数时会使用参的值，没有则使用这个值
			user.setAge(20);
			map.put("abc",user);
		}

		@PostMapping("/login")
		public String login(@ModelAttribute("abc"),User user){
			//这是取的值是处理后的值
		}


### 文件上传 ###
	1. 不使用sprinMVC的方式上传文件
	//页面前提配置
	1. form表单的enctype属性必须是multipart/form-data	
		//默认是application-form utlencoded
		enctype 是表单请求正文类型

	2. method 属性必须是post
	3. input选择域必须提供 <input type="file" name="fileName"/> 
		
	<form action="/user/upload1" method="post" enctype="multipart/form-data">
    <input type="file" name="uplaod">
    <input type="submit" value="上传">
	</form>


	导入依赖
	 <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.3</version>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.6</version>
    </dependency>

	控制层代码

	`
	 public String uplaod1(HttpServletRequest request) throws Exception {
        //创建文件目录
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        //判断目录是否存在，如果不存在就创建
        File file = new File(path);
        if(!file.exists()){
           file.mkdirs();
        }
        //1. 创建文件上传项
        ServletFileUpload upload = new ServletFileUpload(new DiskFileItemFactory());
        //2. 解析request,获取文件项
        List<FileItem> items = upload.parseRequest(request);

        //遍历
        for (FileItem item : items) {
            //不是普通表单
            if(!item.isFormField()){
                //如果不是文件项
                //生成文件名称
                String fileName=item.getName();
                String uuid = UUID.randomUUID().toString().replace("-", "");
                fileName=uuid+"_"+fileName;
                item.write(new File(path,fileName));
                //删除临时文件
                item.delete();
            }
        }
        return "success";
    }
	`
	2. 使用springMVC上传文件
	
		spring-mvc配置文件解析器
		 <!--配置上传文件解析器-->
		    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		        <property name="maxInMemorySize" value="102400" />
		    </bean>
				
		* 前端代码
			<form action="/user/upload2" method="post" enctype="multipart/form-data">
			    <input type="file" name="upload">
			    <input type="submit" value="上传">
			</form>

		后端代码 
	* MultipartFile upload 的参数名称必须与form表单中的name属性值一致
			
		  public String upload2(HttpServletRequest request,MultipartFile upload) throws IOException {
	        //创建文件目录
	        String path = request.getSession().getServletContext().getRealPath("/uploads/");
	        //判断目录是否存在，如果不存在就创建
	        File file = new File(path);
	        if(!file.exists()){
	            file.mkdirs();
	        }
	        System.out.println("上傳對象"+upload);
	        String fileName=upload.getOriginalFilename();
	        String uuid = UUID.randomUUID().toString().replace("-", "");
	        fileName=uuid+"_"+fileName;
	        upload.transferTo(new File(path,fileName));
	        return "success";
	    }



### 跨服务器文件上传 ###

	1. 导入依赖
		 <dependency>
		      <groupId>com.sun.jersey</groupId>
		      <artifactId>jersey-core</artifactId>
		      <version>1.19</version>
		    </dependency>
		    <dependency>
		      <groupId>com.sun.jersey</groupId>
		      <artifactId>jersey-client</artifactId>
		      <version>1.19</version>
		   </dependency>

	* controller代码
	 public String upload3(MultipartFile upload) throws IOException {
        String fileName=upload.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replace("-", "");
        fileName=uuid+"_"+fileName;
        //創建客戶端對象
        Client client = Client.create();
        //建立連接 ,服务器目录
        String path="http://localhost:8090/files/";
        WebResource webResource = client.resource(path + fileName);
        //上傳文件
        webResource.put(upload.getBytes());

        return "success";
    }

	* 错误403，在tomcat中配置 \conf目录下web.xml中添加参数，约100行
	添加
		<init-param>
		<param-name>readonly</param-name>
		<param-value>false</param-value>
		</init-param>
	
	//完整如下
	 <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
		
		<init-param>
		<param-name>readonly</param-name>
		<param-value>false</param-value>
		</init-param>
		
        <load-on-startup>1</load-on-startup>
    </servlet>

	* 409 上传到服务器的文件目录是否正确，或者不存在，如果不存在，webapps中创建即可













