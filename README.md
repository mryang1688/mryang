##spring+springmvc+mybatis整合心得##



 **1、spring框架** (框架的核心)

	*1.导入jar（10个jar）
		*基础4个核心包+1个依赖包：
			* core		---  核心包
			* beans     ---  管理对象
				*依赖包：org.apache.commons.logging.1.1.1.jar---日志包
			* context	 --- 上下文
			* expression --- 表达式
			
		*事务包 （2）
			*jdbc ---  连接
			*tx	  ---  事务
				

		*切面包	（3）
			*Aop --- 面向切面编程
			   *依赖包：
				  *aop联盟
				  *.org.aspectJ.weaver1.6.8.jar

---
	*2.配置文件
		*applicationContext.xml/beans.xml
		文件位置：
				*1.位于源代码包【Sourece folder】
				*2.位于WEB-INF/下
		写法：
				*1.读取配置文件：db.properties
					*例：<context:property-placeholder location="classpath:applicationContext.xml" />
				
				*配置数据源
					*数据库驱动
					*数据库连接
					*数据库用户名
					*数据库密码
						*最大连接数maxIdle
						*最小连接数minIdle
						*初始化连接数"initialSize"
							**例：<bean id="dataSource" class="">
								  <property name="" ref="" value=""/> 
								</bean> 
			
				*事务管理器
							**例：<bean id="transactionManager" class="">
									<property name="datasource" ref="dataSource">
								</bean>

				*[事务]通知（增强方法）
						
							**例：<tx:advice id="txAdvice" transaction-Manager="transactionManager"  >
									<!--传播行为-->
								<tx:method name="save" propagation="REQUIRED" />
						 		<tx:method name="insert" propagation="REQUIRED" />
						 		<tx:method name="create" propagation="REQUIRED" />
						 		<tx:method name="delete" propagation="REQUIRED" />
						 		<tx:method name="update" propagation="REQUIRED" />
						 		<tx:method name="find" propagation="SUPPORTS" />
				
				*切面
							
						**例：<aop:config>
								<aop:advisor advice-ref="txAdvice"/>
	 							<aop:pointcut expression="" id=""/>

				*扫描mapper包
						*扫描mybatis持久化的映射文件
							**例: <context:component-scan base-package="cn.javabs.mapper"/>

			    *扫描services包
							**例：<context:component-scan base-package="cn.javabs.service"/>

						
										
---
		

	*3.配置文件
		* web.xml
			*1. 找到配置文件的位置<!-- 指定spring配置文件的位置 -->
  			<context-param>
  				<param-name>contextConfigLocation</param-name>
  				<param-value>classpath:applicationContext.xml</param-value>
  			</context-param>
			*2. 配置监听器  		<!-- 监听器  -->
  			<listener>	
  				<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  			</listener>


---

	43.配置文件
		* db.properties[根据不同的数据源去写配置]
			*1. 数据驱动
  			*2. 数据链接
					**mysql：
						jdbc:mysql://192.168.50.9:3306/DBName
			*3. 数据库用户名
			*4. 数据库密码


---

**2、mybatis框架**

	*1.导入jar包（13个jar包）
		*核心包：mybatis-3.4.2.jar

		*依赖包：
				ant-1.9.6.jar
				ant-launcher-1.9.6.jar
				asm-5.1.jar
				cglib-3.2.4.jar
				commons-logging-1.2
				javassist-3.21.0-GA
				log4j-1.2.17.jar
				log4j-api-2.3.jar
				log4j-core-2.3.jar
				ognl-3.1.12.jar
				slf4j-api-1.7.22.jar
				slf4j-log4j12-1.7.22.jar

		
---
	*2.配置文件
			*mybatis-config.xml

				**起别名【在写动态传参数类型时，可以简写类名】
					<typeAliases>
			  			<package name="cn.javabs.ssm.po"/>
			  		</typeAliases>

			
---
	*3.持久化类的生成器
			* 生成持久化类 + 配置文件 + dao接口
			
---
 与数据库进行连接的辅助jar包
---
**mybatis与spring整合的中间jar包（1个）**
	
	* mybatis-spring-1.3.1.jar

---
**mysql数据库的驱动jar包（1个）**
	
	* mysql-connector-java-5.1.8.jar

---
**数据库的数据源连接池jar包-dbcp（2个）**[数据源二选一]
	
	 * commons-dbcp2-2.1.1.jar
	 * commons-pool2-2.4.2.jar

---
**数据库的数据源连接池jar包-c3p0（1个）**[数据源二选一]
	
	 * c3p0-0.9.1.2.jar

---

**3、SpringMVC框架**

	*1.导入jar包
			* webmvc.jar
			* web.jar
			

---
	*2.配置文件
		web.xml
			配置前端控制器
				<!-- 前端控制器 :springmvc配置 -->
				如果不配置初始化参数  框架默认配置文件名为：ServletName-servlet.xml
											举例：以下若不配置初始化，默认名称为：spring-servlet.xml
				  	<servlet>
				  		<servlet-name>spring</servlet-name>
				  		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
				  		<init-param>
				  			<param-name>contextConfigLocation</param-name>
				  			<param-value>classpath:springmvc-config.xml</param-value>
				  		</init-param>
				  		<!-- 自动加载前端控制器 -->
				  		<load-on-startup>1</load-on-startup>
				  	</servlet>
				  	
				  	<servlet-mapping>
				  		<servlet-name>spring</servlet-name>
				  		<url-pattern>/</url-pattern>
				  	</servlet-mapping>

---


				为了避免中文输出乱码：[建议配置编码过滤器]
						<!-- 编码过滤器  -->
						  	<filter>
						  		<filter-name>encoding</filter-name>
						  		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
						  		<init-param>
						  			<param-name>encoding</param-name>
						  			<param-value>utf-8</param-value>
						  		</init-param>
						  	</filter>
  	
						  	<filter-mapping>
						  		<filter-name>encoding</filter-name>
						  		<url-pattern>/*</url-pattern>
						  	</filter-mapping>

---

				注意：拦截的是所有  一般开发使用的是“/”代表仅不拦截.jsp的后缀
					 如果想不拦截其他的静态资源，需要在springmvc-config.xml中加入以下标签:
						<!-- 配置静态资源的访问映射路径  此配置中的文件  将不会被前台的 控制器进行拦截 -->
							<mvc:resources location="/js/" mapping="/js/**"/>
							<mvc:resources location="/css/" mapping="/css/**"/>
							<mvc:resources location="/fonts/" mapping="/fonts/**"/>
							<mvc:resources location="/images/" mapping="/images/**"/>

---
	*2.配置文件
		spring-config.xml
			*1.配置扫描器[controller包]
					例：<context:component-scan base-package="cn.javabs.ssm.controller"/>
			*2.加载注解驱动
					例：<mvc:annotation-driven/>
			*3. 配置静态资源的访问映射路径  此配置中的文件  将不会被前台的 控制器进行拦截 
						<mvc:resources location="/js/" mapping="/js/**"/>
						<mvc:resources location="/css/" mapping="/css/**"/>
						<mvc:resources location="/fonts/" mapping="/fonts/**"/>
						<mvc:resources location="/images/" mapping="/images/**"/>
			*4.配置视图解析器
						<bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
							<!--前缀-->
							<property name="prefix" value="/WEB-INF/jsp/"/>
							<!--后缀-->
							<property name="suffix" value=".jsp"/>
						</bean>

				
