# SpringBoot Code Snippets

## 1. Getting the name of the url for war deployed microservice

| Time Complexity | O(N) [ ] |  O(N-log(N)) [ ] | O(N**2) [ ] | Untested [ x ]  |
|-----------------|----------|------------------|------------|-------------|


```java
    @SpringBootApplication
    public class ExpenseMasterApplication implements ServletContextAware {
	
        private static ServletContext servletContext;

        public static void main(String[] args) {
            SpringApplication.run(ExpenseMasterApplication.class, args);
        }

        @Override
        public void setServletContext(ServletContext servletContext) {
            ExpenseMasterApplication.servletContext = servletContext;		
        }
        
        @Component
        public static class MyServlet extends HttpServlet {

            @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                resp.getWriter().write("Microservice started. Context path: " + req.getContextPath());
            }
        }

        @EventListener(ApplicationReadyEvent.class)
        public void onApplicationReadyEvent() {
            System.err.println("Microservice started. Context path: " + getContextPath());
            // You can also log the context path using your preferred logging framework
        }

        private String getContextPath() {
            return servletContext.getContextPath();
        }
    }
```

## 2. Logpath xml configuration

| Time Complexity | O(N) [ ] |  O(N-log(N)) [ ] | O(N**2) [ ] | Untested [ x ]  |
|-----------------|----------|------------------|------------|-------------|


```xml
<configuration scan="true">
	
	<variable file="${HOME}/properties/log_properties.properties" scope="context" />	
	<include resource="org/springframework/boot/logging/logback/defaults.xml" />
	<timestamp key="date" datePattern="yyyy-MM-dd" />
	
	<appender name="FILE-ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>${LOGPATH_REGISTRY}/app-${date}.log</file>
		<withJansi>true</withJansi>
		
		<rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
			<fileNamePattern>${LOGPATH_REGISTRY}/archive/manager-%d{yyyyMMdd}-%i.log </fileNamePattern>
			<maxFileSize>10MB</maxFileSize>
			<totalSizeCap>20MB</totalSizeCap>
			<maxHistory>60</maxHistory>
		</rollingPolicy>
		
		<encoder>
			<pattern>%date %level [%thread] %logger{10} [%file:%line] -%kvp- %msg%n</pattern>
		</encoder>
	</appender>
	
	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
		<layout class="ch.qos.logback.classic.PatternLayout">
			<pattern>%d{HH:mm:ss.SSS} [%thread] %highlight(%-5level) %green(%logger{40}) -%kvp- %msg%n</pattern>
		</layout>
	</appender>
	
	<logger name="com.app.logger" level="trace" additivity="false">
		<appender-ref ref="FILE-ROLLING" />
	</logger>
	
	<logger name="com.app.logger" level="debug" additivity="false">
		<appender-ref ref="CONSOLE" />
	</logger>
	
	<root level="error">
		<appender-ref ref="CONSOLE" />
		<appender-ref ref="FILE-ROLLING" />
	</root>
		
	<root level="info">
		<appender-ref ref="CONSOLE" />
		<appender-ref ref="FILE-ROLLING" />
	</root>
</configuration>
```

## 3. JDBC application properties

| Time Complexity | O(N) [ ] |  O(N-log(N)) [ ] | O(N**2) [ ] | Untested [ x ]  |
|-----------------|----------|------------------|------------|-------------|


```sh
server.port= 9001

spring.datasource.url=jdbc:mysql://localhost:3306/jdbc1
spring.datasource.username=root
spring.datasource.password=admin
logging.level.web=DEBUG
logging.file.name=application1.log
```
