## Spring Project 생성
1. Java Project 생성
2. 프로젝트 우클릭 - configure - Convert to Maven Project
3. 프로젝트 우클릭 - Spring - Add Spring Project Nature

<br>
<br>

## Spring Module 설치
* pom.xml 파일에 dependency 추가:
    - https://mvnrepository.com 에서 spring context module 검색 후 복사 붙여넣기

※ 참고<br>
[STS3 설치](https://life-with-coding.tistory.com/369)

```xml
<!-- pom.xml -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>SpringPjt</groupId>
  <artifactId>SpringPjt</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <dependencies>
  	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>5.2.12.RELEASE</version>
	</dependency>
 </dependencies>
  <build>
    <sourceDirectory>src</sourceDirectory>
    <plugins>
      <plugin>
        
        <artifactId>maven-compiler-plugin</artifactId>
		<version>3.8.1</version>
		<configuration>
			<source>1.8</source>
			<target>1.8</target>
			<encoding>utf-8</encoding>
		</configuration>
        
        
      </plugin>
    </plugins>
  </build>
</project>
```

