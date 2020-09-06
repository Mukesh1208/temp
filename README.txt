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
