<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.mkyong.core</groupId>
	<artifactId>WicketExamples</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>WicketExamples</name>
	<url>http://maven.apache.org</url>

	<dependencies>

		<!--<version>1.4.17</version> -->

        <!-- https://mvnrepository.com/artifact/org.apache.wicket/wicket -->
        <dependency>
            <groupId>org.apache.wicket</groupId>
            <artifactId>wicket</artifactId>
           <!-- <version>6.15.0</version> -->
            <version>6.15.0</version>
            <type>pom</type>
        </dependency>


        <!-- slf4j-log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.7</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
		</dependency>

	</dependencies>

	<build>
		<finalName>WicketExamples</finalName>

		<resources>
			<resource>
				<directory>src/main/java</directory>
				<excludes>
					<exclude>**/*.java</exclude>
				</excludes>
			</resource>
		</resources>

		<plugins>
			<plugin>
				<inherited>true</inherited>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
					<debug>true</debug>
				</configuration>
			</plugin>

			<plugin>
				<groupId>org.eclipse.jetty</groupId>
				<artifactId>jetty-maven-plugin</artifactId>
				<version>9.4.31.v20200723</version>
				<configuration>
					<webAppSourceDirectory>src/webapp</webAppSourceDirectory>
					<scanIntervalSeconds>10</scanIntervalSeconds>
				</configuration>
			</plugin>
		</plugins>



	</build>

	<pluginRepositories>
	<pluginRepository>
		<id>mortbay-repo</id>
		<name>mortbay-repo</name>
		<url>http://www.mortbay.org/maven2/snapshot</url>
	</pluginRepository>
</pluginRepositories>
</project>



web.xml

<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
	version="2.4">
	<display-name>Wicket Web Application</display-name>

	<context-param>
		<param-name>configuration</param-name>
		<param-value>development</param-value>
	</context-param>

	<filter>
		<filter-name>wicket.wicketTest</filter-name>
		<filter-class>org.apache.wicket.protocol.http.WicketFilter</filter-class>
		<init-param>
			<param-name>applicationClassName</param-name>
			<param-value>com.mkyong.WicketApplication</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>wicket.wicketTest</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app>

log4j.properties


# Root logger option
log4j.rootLogger=DEBUG, stdout, file

# Redirect log messages to console
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Redirect log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=${catalina.home}/logs/mywicketapp.log
log4j.appender.file.MaxFileSize=5KB
log4j.appender.file.MaxBackupIndex=5
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n


WicketApplication.java

package com.mkyong;

import org.apache.wicket.protocol.http.WebApplication;

import com.mkyong.user.UserPage;

public class WicketApplication extends WebApplication {

	@Override
	public Class<UserPage> getHomePage() {
		return UserPage.class; // return default page
	}

}


SuccessPage.java

package com.mkyong.user;

//import org.apache.wicket.PageParameters;
import org.apache.wicket.request.mapper.parameter.PageParameters;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.WebPage;

public class SuccessPage extends WebPage {

	public SuccessPage(final PageParameters parameters) {

		String username = "";
		
		if(parameters.get("username")!= null){
			username = parameters.get("username").toString();
		}
		
		final Label result = new Label("result", "Username : " + username);
		add(result);
		
	}
}


SuccessPage.html
<html>
<body>
	<h1>Wicket TextBox Example - SuccessPage.html</h1>

	<label wicket:id="result"></label>

</body>
</html>

UsernameValidator.java

package com.mkyong.user;

import org.apache.wicket.validation.CompoundValidator;
import org.apache.wicket.validation.validator.PatternValidator;
import org.apache.wicket.validation.validator.StringValidator;

public class UsernameValidator extends CompoundValidator<String> {
	
	private static final long serialVersionUID = 1L;

	public UsernameValidator() {
	
		add(StringValidator.lengthBetween(5, 15));
		
		//Match characters and symbols in the list, a-z, 0-9, underscore, hyphen
		add(new PatternValidator("[a-z0-9_-]+"));
	
	}
}

UserPage.java

package com.mkyong.user;

//import org.apache.wicket.PageParameters;
import org.apache.wicket.IRequestListener;
import org.apache.wicket.RequestListenerInterface;
import org.apache.wicket.ajax.AjaxRequestTarget;
import org.apache.wicket.ajax.markup.html.form.AjaxButton;
import org.apache.wicket.request.IRequestParameters;
import org.apache.wicket.request.Request;
import org.apache.wicket.request.cycle.RequestCycle;
import org.apache.wicket.request.http.flow.AbortWithHttpErrorCodeException;
import org.apache.wicket.request.mapper.parameter.PageParameters;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.Model;

import javax.servlet.http.HttpServletRequest;
import java.util.logging.Logger;

public class UserPage extends WebPage implements MyRequestHandler {
	private static final Logger log = Logger.getLogger(UserPage.class.getName());
	Form<?> form;
	public UserPage(final PageParameters parameters) {
		add(new FeedbackPanel("feedback"));

		final TextField<String> username = new TextField<String>("username",
				Model.of(""));
		username.setRequired(true);
		username.add(new UsernameValidator());

		 form = new Form<Void>("userForm") {

		 	protected String getMethod() {
		 		return "POST";
			}

		 	@Override
			protected MethodMismatchResponse onMethodMismatch() {
				System.out.println("AAAAAAAAAAAAA");
				log.info("AAAAAAAAAAAAA");
			return MethodMismatchResponse.ABORT;
		 }


			@Override
			protected void onSubmit() {
				log.info(">>1");
				final String usernameValue = username.getModelObject();
				log.info(">>2");
				PageParameters pageParameters = new PageParameters();
				log.info(">>3");
				pageParameters.add("username", usernameValue);
				log.info(">>4");
				setResponsePage(SuccessPage.class, pageParameters);
				log.info(">>5");

			}

		};

		AjaxButton nextButton = new AjaxButton("goToNextPage", Model.of("ajaxmodel")) {
			private static final long serialVersionUID = 7617100028358027942L;

			@Override
			public void onSubmit(AjaxRequestTarget target, Form<?> arg1) {
				System.out.println("AJAX BUTTON RUNNING");
				Form.MethodMismatchResponse result = checkMethodMismatch();
				System.out.println("AJAX BUTTON RESULT=" + result);
				if (result == Form.MethodMismatchResponse.ABORT) {
					System.out.println("GET --abort--");

					throw new AbortWithHttpErrorCodeException(403);

				}


			}
		};
		nextButton.setDefaultFormProcessing(false);
		form.add(nextButton);

		add(form);
		form.add(username);



	}

	@Override
	public void onRequest() {
		System.out.println("Running onRequest");
		Request request = RequestCycle.get().getRequest();
		String actualMethod = ((HttpServletRequest) request.getContainerRequest()).getMethod();
		if (!"post".equalsIgnoreCase(actualMethod)) {
			throw new IllegalArgumentException("Incorrect request. Method should be POST");
		} else {
			System.out.println("IT IS GOOD WE NEED TO CONTINUE");
		}

		final RequestListenerInterface rli = MyRequestHandler.INTERFACE;

		PageParameters pageParameters = new PageParameters();
		log.info(">>!!!!");
		pageParameters.add("username", "overriden");

		form.setResponsePage(SuccessPage.class, pageParameters);


	}

	private Form.MethodMismatchResponse checkMethodMismatch()
	{

		System.out.println("@@CSRF GET checkMethodMismatch()");
		String desiredMethod = Form.METHOD_POST;  //POST=post=desiredmeth
		System.out.println("@@CSRF desiredMethod checkMethodMismatch() = "+desiredMethod);
		String actualMethod = ((HttpServletRequest)getRequest().getContainerRequest()).getMethod(); //post=actual==get
		System.out.println("@@CSRF actualMethod from request checkMethodMismatch() = "+actualMethod);
		//actualMethod = "get";
		//System.out.println("@@CSRF actualMethod tampered checkMethodMismatch() = "+actualMethod);
		if (!desiredMethod.equalsIgnoreCase(actualMethod))
		{
			System.out.println("@@CSRF ABORT checkMethodMismatch()");

			return Form.MethodMismatchResponse.ABORT;
		} else {
			System.out.println("@@CSRF CONTINUE checkMethodMismatch()");
			return Form.MethodMismatchResponse.CONTINUE;
		}
	}
}


UserPage.html

<html>
<head>
<style>
label {
	background-color: #eee;
	padding: 4px;
}

.feedbackPanelERROR {
	color: red;
}
</style>
</head>
<body>
	<h1>Wicket TextBox Example - UserPage.html</h1>

	<div wicket:id="feedback"></div>
	<form wicket:id="userForm">
		<p>
			<label>Username</label>: <input wicket:id="username" type="text"
				size="20" />
		</p>
		<!--<input type="submit" value="Register" />-->
		<button wicket:id="goToNextPage" type="submit"></button>

	</form>

</body>
</html>



