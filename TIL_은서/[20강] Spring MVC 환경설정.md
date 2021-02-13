# STS4에 Dynamic Web Project 없을 때

#### Help >  Install New Software에서 아래 세 가지 설치

#### 1. Eclipse Java EE Developer Tools

#### 2. Eclipse Java Web Developer Tools

#### 3. Eclipse Web Developer Tools



# Dynamic Web Project 생성

#### 1. Dynamic Web Project 생성

#### 생성시 'Generate web.xml development descriptor' 체크



#### 2. Maven Project로 변환

#### 생성 후 Configure > Convert to Maven Project



#### 3. Spring Project Nature 추가

#### Spring Tools > Add Spring Project Nature



#### 4. 기존에 만들었던 파일 복사



# Spring MVC 설정

#### 1. Spring MVC 의존성 주입



#### 2. web.xml에 ContextLoaderListener 클래스 설정

##### web.xml

```xml
  <!-- needed for ContextLoaderListener -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:/config/beans.xml</param-value>
	</context-param>

	<!-- Bootstraps the root web application context before servlet initialization -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

> Web application은 JVM(Java Virtual Machine: 자바가상머신) 위에 Web Container(Tomcat)에서 동작하게 된다
>
> **🚨 ContextLoaderListener의 역할**
>
> 이 Web Container가 기존에 작성했던 **beans.xml 파일의 위치를 알려주어 파싱할 수 있도록** 해줌



#### 3. Spring Beans 파일 생성(beans-web.xml)

🚨 생성 후 namespace에는 beans, context, mvc 선택 



#### 4. web.xml에 DispatcherServlet 클래스 설정

##### web.xml

```xml
	<!-- The front controller of this Spring Web application, responsible for handling all application requests -->
	<servlet>
		<servlet-name>springDispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:/config/beans*.xml</param-value> ✔ 입력
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map all requests to the DispatcherServlet for handling -->
	<servlet-mapping>
		<servlet-name>springDispatcherServlet</servlet-name>
		<url-pattern>url</url-pattern>
	</servlet-mapping>
```

> beans.xml과 beans-web.xml 파일 모두 인식할 수 있게하기 위해 설정