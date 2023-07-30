# 1. Logstash-Logback
```sh
<?xml version="1.0" encoding="UTF-8"?>

<configuration scan="true">

	<include
		resource="org/springframework/boot/logging/logback/defaults.xml" />
	<timestamp key="date" datePattern="yyyy-MM-dd" />

	<appender name="FILE-ROLLING"
		class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>/home/kp/logs/logstash-logback/app-${date}.log</file>
		<withJansi>true</withJansi>

		<rollingPolicy
			class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
			<fileNamePattern>/home/kp/logs/logstash-logback/archive/manager-%d{yyyyMMdd}-%i.log
			</fileNamePattern>
			<maxFileSize>10MB</maxFileSize>
			<totalSizeCap>20MB</totalSizeCap>
			<maxHistory>60</maxHistory>
		</rollingPolicy>

		<encoder class="net.logstash.logback.encoder.LogstashEncoder" >
			<!-- <pattern>%date %level [%thread] %logger{10} [%file:%line] -%kvp-
				%msg%n</pattern> -->
		</encoder>
	</appender>

	<appender name="CONSOLE"
		class="ch.qos.logback.core.ConsoleAppender">
		<layout class="ch.qos.logback.classic.PatternLayout">
			<pattern>%d{HH:mm:ss.SSS} [%thread] %highlight(%-5level)
				%green(%logger{36}) -%kvp- %msg%n</pattern>
		</layout>
	</appender>

<!-- class="net.logstash.logback.appender.LogstashTcpSocketAppender" -->

	<appender name="stash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
       <destination>127.0.0.1:5044</destination>
        <!-- encoder is required -->
        <encoder class="net.logstash.logback.encoder.LogstashEncoder" >
        	<!-- <pattern>[]%date %level [%thread] %logger{10} [%file:%line] -%kvp-
				%msg%n</pattern> -->
        </encoder>
    </appender>

	<springProfile name="default">
		<root level="info">
			<!-- <appender-ref ref="CONSOLE" /> -->
			<appender-ref ref="FILE-ROLLING" />
			<appender-ref ref="stash" />
		</root>
	</springProfile>

</configuration>
```
