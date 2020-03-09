
###IDEA搭建ssm框架
	1.创建maven项目
		选择Create from archetype
		orgapache.cocoon:maven-archetype-webapp
		点击next后出现如下填写GroupId和ArtifactId在点击next直至finish

###2.在项目中的main文件右键新建一个文件夹
		1.新建一个java文件夹，选中右键Mark Directory as 文件夹标标记为Sources Root	
		2.新建一个ressources 选中右键Mark Directory as 文件夹标记为ReSources Root
		3.在java包下创建对应package

###3.在pom.xml中添加
	

	<properties>
		<!-- 版本设置-->
		<webVersion>3.0</webVersion>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<spring-version>4.3.10.RELEASE</spring-version>
	</properties>	


	<dependencies>

		<!-- 导入servlet依赖 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<!-- provided：只在开发的时候使用，导出为war包后不编译这个jar包 -->
			<scope>provided</scope>
		</dependency>

		<!-- JSTL标签 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- 面向切面 -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.8.10</version>
		</dependency>
		<!-- Spring 依赖包 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<!-- spring MVC -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<!-- spring web MVC -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring-version}</version>
		</dependency>

		<!-- mybatis相关的包 -->
		<!-- Mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.4</version>
		</dependency>
		<!-- mybatis -spring整合包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.1</version>
		</dependency>

		<!--aspectjweaver包:面向切面要用到的包 -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.8.5</version>
		</dependency>
		<!-- Junit 测试 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
		</dependency>
		<!-- c3p0 -->
		<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5.2</version>
		</dependency>

		<!-- 日志log4j -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.25</version>
			<scope>test</scope>
		</dependency>
		<!-- mysql驱动包 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.38</version>
		</dependency>

		<!-- json格式 -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.6.3</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.6.3</version>
		</dependency>

	</dependencies>


###4. src/main/webapp/WEB-INF  web.xml文件中添加 

	**发生错误添加属性 
		<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		         xmlns="http://java.sun.com/xml/ns/javaee"
		         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
		         id="WebApp_ID" version="3.0">
	**

	<!-- 监听器加载spring -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:application-Context.xml</param-value>
	</context-param>

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
	  <!-- 处理PUT提交参数(只对基础表单生效) -->
	  <filter>
	    <filter-name>httpPutFormContentFilter</filter-name>
	    <filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
	  </filter>

	<!-- springMVC前端控制器 -->
	<servlet>
		<servlet-name>springMVC</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!--初始化加载spring-MVC.xml -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring-mvc.xml</param-value>
		</init-param>
		<!-- 优先启动这个servlet -->
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springMVC</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>


###5.src/main/resources 新建 application-Context.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<!-- 扫描注解 -->
	<context:component-scan base-package="cn.yf.service.impl"></context:component-scan>

	<!-- 引入本地properties文件 -->
	<context:property-placeholder location="classpath:db.properties" />
	
	<!-- c3p0数据源 -->
	<bean id="dataSourceC3P0" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<property name="driverClass" value="${driverClass}"></property>
		<property name="jdbcUrl" value="${jdbcUrl}"></property>
		<property name="user" value="${user}"></property>
		<property name="password" value="${password}"></property>
	</bean>


	<!-- 配置mybvatis的SqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSourceC3P0"></property>
		<!-- 配置加载mybatis的配置文件 -->
		<property name="configLocation" value="classpath:mybatis-config.xml"></property>

		<!-- XML文件在哪里 -->
		<property name="mapperLocations"
			value="classpath:mappers/*.xml" />


		<!-- 配置mybatis实体别名的扫描包 -->
		<property name="typeAliasesPackage" value="cn.yf.entity"></property>
	</bean>

	<!-- 配置mybatis扫描mapper，自动实现dao层接口 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 配置扫描的路径 -->
		<property name="basePackage" value="cn.yf.mapper"></property>
	</bean>
	<!-- 配置事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSourceC3P0"></property>
	</bean>
	<!-- 配置事务传播属性 -->
	<tx:advice id="tx" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="query*" propagation="REQUIRED" read-only="true" />
			<tx:method name="find*" propagation="REQUIRED" read-only="true" />
			<tx:method name="add*" propagation="REQUIRED" />
			<tx:method name="save*" propagation="REQUIRED" />
			<tx:method name="update*" propagation="REQUIRED" />
			<tx:method name="delete*" propagation="REQUIRED" />
			<tx:method name="*" propagation="REQUIRED" />
		</tx:attributes>
	</tx:advice>
	<!-- 配置AOP -->
	<aop:config>
		<!-- 指定service的实现类上加事务 -->
		<aop:pointcut expression="execution(* cn.yf.service.impl.*.*(..))"
			id="pointcut" />
		<aop:advisor advice-ref="tx" pointcut-ref="pointcut" />
	</aop:config>
	</beans>
	

###6. src/main/resources 新建文件 db.properties

	#orcale
	#driverClass=oracle.jdbc.driver.OracleDriver
	#jdbcUrl=jdbc:oracle:thin:@localhost:1521:xe
	#user=realzx06
	#password=realzx06
	#mysql
	#hibernate.dialect=org.hibernate.dialect.MySQLDialect
	driverClass=com.mysql.jdbc.Driver
	validationQuery=SELECT 1
	jdbcUrl=jdbc:mysql://localhost:3306/webank?useUnicode=true&characterEncoding=UTF-8
	user=root
	password=123

###7.src/main/resources 新建文件  mybatis-config.xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	  PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
	  "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	</configuration> 

###8.src/main/resources 新建文件  spring-mvc.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<!-- 开启注解扫描 -->
	<mvc:annotation-driven></mvc:annotation-driven>
	<!-- 配置注解扫描的路径 -->
	<context:component-scan base-package="cn.yf.controller"></context:component-scan>
	<!-- 配置视图转换器 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 设置视图转化的前缀 -->
		<property name="prefix" value="/WEB-INF/"></property>
		<!-- 设置视图转化的后缀 -->
		<property name="suffix" value=".jsp"></property>
	</bean>
	<!-- 防止静态资源被拦截 -->
	<mvc:default-servlet-handler />
	<!-- JSON处理转换器 -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <ref bean="jsonHttpMessageConverter" />
            </list>
        </property>
    </bean>
    <bean id="jsonHttpMessageConverter"
          class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <property name="supportedMediaTypes">
            <list>
                <value>application/json;charset=UTF-8</value>
            </list>
        </property>
    </bean>
	</beans>



###9. 可选src/main/resources 新建文件  Log4j.properties

	# Set root category priority to INFO and its only appender to CONSOLE.
	# 输出日志等级log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
	log4j.rootCategory=debug, CONSOLE, LOGFILE
	
	# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
	log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE
	
	# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
	log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
	log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
	log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
	
	#  输入到文件LOGFILE is set to be a File appender using a PatternLayout.
	# log4j.appender.LOGFILE=org.apache.log4j.FileAppender
	# log4j.appender.LOGFILE.File=d:\axis.log
	# log4j.appender.LOGFILE.Append=true
	# log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
	# log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n



###展示层测试
	右上运行按钮旁边找到下拉选择Edit Configuratios..
		1.点击Defaults找到TomcatServer
			Local
				Server
			Configure...选择tomcat服务器

		2.点击Deployment
			点击+号Atifact，部署项目 ok
		
	如果页面乱码在头部加上<%@ page language="java" contentType="text/html; charset=UTF-8"  pageEncoding="UTF-8"%>


###10.src/main/resources 新建文件夹 mappers，创建文件持久层UserMapper.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.yf.ssm2.mapper.UserMapper">
	
	  <!-- Integer findByName(String typeName); -->
	  
	   <select id="findCount" resultType="java.lang.Integer">
	   
	    select
	    	 COUNT(*)
	    from
	    	 user
	  </select>
	  
	</mapper>

###10.1 cn.yf.mapper文件创建持久层映射接口 UserMapper 声明一个抽象方法
	 Integer findCount();
	

###10.2 main 创建文件夹test 标记为 Test Source Root
	
	
###11.持久层测试单元
	
	public class TestCase {

	ClassPathXmlApplicationContext ctx;
	SysUserMapper mapper;
	@Before
	public void init() {
		System.out.println("init");
		ctx = new ClassPathXmlApplicationContext("application-Context.xml");
		mapper=ctx.getBean("sysUserMapper",SysUserMapper.class);
		
	}
	@After
	public void destroy() {
		
		System.out.println("destroy");
	}
	@Test
	public void test() {
		System.out.println("OK"+mapper.findCount());
	}
}

###12.mybatis反向生成 pom.xml添加依赖

	<!-- myBatis反向生成javaBean -->
		<dependency>
		    <groupId>org.mybatis.generator</groupId>
		    <artifactId>mybatis-generator-core</artifactId>
		    <version>1.3.5</version>
		</dependency>

###13.src/main/resources 新建文件 generatorConfig.xml
	
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE generatorConfiguration
	PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
	"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
	
	<generatorConfiguration>

	<context id="mysql" targetRuntime="MyBatis3Simple"
		defaultModelType="flat">
		<!-- 注释生成器 -->
		<commentGenerator>
 			<!-- 逆向生成时，flase生成注释 -->
			<property name="suppressAllComments" value="false" />
			<property name="suppressDate" value="true" />
		</commentGenerator>

		<!-- 数据库连接相关信息 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/hotel"
			 userId="root"
			password="123">
		</jdbcConnection>

		<!-- Java类型解析 -->
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- 指定实体生成的位置 -->
		<javaModelGenerator targetPackage="cn.tedu.hotel.entity"
			targetProject="src\main\java">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>

		<!-- 指定映射文件生成的位置 -->
		<sqlMapGenerator targetPackage="mappers" targetProject="src\main\resources">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>

		<!-- 映射信息对应的接口生成位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="cn.tedu.hotel.mapper" targetProject="src\main\java">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>

		<!-- 定义表以及生成的实体类、dao类、映射文件等信息 -->
		<!-- <table tableName="tb_door" domainObjectName="Door" mapperName="IDoorMapper" /> -->
		<table tableName="hotel_order---" domainObjectName="HotelOrder" mapperName="IHotelOrderMapper" />
		
	
	</context>
	</generatorConfiguration>


###14.测试文件中创建反向生成执行类  GeneratorTest
	import java.io.InputStream;
	import java.util.ArrayList;
	import java.util.List;
	
	import org.mybatis.generator.api.MyBatisGenerator;
	import org.mybatis.generator.config.Configuration;
	import org.mybatis.generator.config.xml.ConfigurationParser;
	import org.mybatis.generator.internal.DefaultShellCallback;
	public class GeneratorTest {
	public static void main(String[] args) throws Exception {
		
		List<String>warnings = new ArrayList<String>();
		boolean overwrite = true;
		//反向生工具GeneratorTest
		InputStream in = GeneratorTest.class.getResourceAsStream("/generatorConfig.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(in);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
		myBatisGenerator.generate(null);
		System.err.println("Generator OK"); 
	}
	}