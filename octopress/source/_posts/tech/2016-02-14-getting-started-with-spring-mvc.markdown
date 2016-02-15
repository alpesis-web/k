---
layout: post_tech
title: "Getting Started with Spring MVC"
date: 2016-02-14 18:04:37 +0800
comments: true
categories: [tech]
tags: [java, maven, tomcat, spring, servlet]
toc: true
---

Steps

- Installations
- Project creation 

## Installations

- Java 8
- Maven 3: a build tool
- Tomcat 8: a web server
- Eclipse: IDE


### Java

```bash
$ sudo add-apt-repository -y ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install -y oracle-java8-installer
$ java -version
```

### Maven

[maven](http://maven.apache.org/download.cgi)

```bash
$ wget http://apache.mirror.anlx.net/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.zip
$ sudo unzip apache-maven-3.3.9-bin.zip
$ sudo mv apache-maven-3.3.9/ /usr/local/maven/
$ vim ~/.bashrc
$ source ~/.bashrc
$ mvn -v
```

`~/.bashrc`

```bash
# maven
export MAVEN_HOME=/usr/local/maven
export PATH=$PATH:$MAVEN_HOME/bin
```

### Tomcat

[Tomcat](http://tomcat.apache.org/download-80.cgi)

```bash
$ wget http://www.mirrorservice.org/sites/ftp.apache.org/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.zip
$ sudo unzip apache-tomcat-8.0.32.zip
$ sudo mv apache-tomcat-8.0.32/ /usr/local/tomcat/
$ cd /usr/local/tomcat/
$ sudo chmod +x bin/*.sh
$ sudo bin/catalina.sh run
$ cd /tmp
$ curl http://localhost:8080
```

### Eclipse

[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/)

## Project Creation

- creating a maven project

### Maven Project

[Maven Getting Started](https://maven.apache.org/guides/getting-started/)

#### mvn

```bash
$ mvn -B archetype:generate \
      -DarchetypeGroupId=org.apache.maven.archetypes \
      -DgroupId=com.mycompany.app \
      -DartifactId=my-app \
      -DarchetypeArtifactId=maven-archetype-quickstart \
      -DinteractiveMode=false \
      -Dpackaging=war
```

folder

```
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
```

#### pom.xml

Dependencies:

- Servlet
- Spring

edit pom.xml

```xml
<properties>
	<java.version>1.8</java.version>
	<spring.version>4.1.6.RELEASE</spring.version>
</properties>
 
<dependencies>
 
	<!-- Servlet API -->
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>javax.servlet-api</artifactId>
		<version>3.1.0</version>
		<scope>provided</scope>
	</dependency>
 
	<!-- Spring Core -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>${spring.version}</version>
	</dependency>
 
	<!-- Spring MVC -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>${spring.version}</version>
	</dependency>
 
</dependencies>
 
<!-- boilerplate code allowing Maven to generate a .war archive without requiring a web.xml file -->
<build>
	<finalName>springwebapp</finalName>
 
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-war-plugin</artifactId>
			<version>2.6</version>
			<configuration>
				<failOnMissingWebXml>false</failOnMissingWebXml>
			</configuration>
		</plugin>
	</plugins>
</build>
```


### Spring MVC

```bash
$ cd src/main/java/com/<groupId>/
$ mkdir config
$ mkdir controller
```

#### Config

```java AppConfig.java
package com.todo.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"com.todo.controller"})
public class AppConfig{
}
```

```java ServletInitializer.java
package com.todo.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class ServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

  @Override
  protected Class<?>[] getRootConfigClasses() {
    return new Class<?>[0];
  }

  @Override
  protected Class<?>[] getServletConfigClasses() {
    return new Class<?>[]{AppConfig.class};
  }

  @Override
  protected String[] getServletMappings() {
    return new String[]{"/"};
  }
}
```

#### Controller

```java HelloController.java
package com.todo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
  @RequestMapping("hi")
  @ResponseBody
  public String hi() {
    return "Hello world.";
  }
}
```


### Run

```bash
$ mvn clean install
$ sudo cp target/springwebapp.war /usr/local/tomcat/webapps/
$ curl http://localhost:8080/springwebapp/hi
```
