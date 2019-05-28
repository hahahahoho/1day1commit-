오랫만에 포스팅입니다. 이번에 국방부 프로젝트 과제에 참여하게 되어 스프링부트 + wildfly + cubrid를 이용하여 프로젝트를 수행하게 되었습니다. 기존에 그냥 사용하던 것에서 처음부터 직접 구성하며 좀 더 효율적으로 프로젝트를 수행하기 위해 공부하는 내용정리하여 올립니다 ^^.

# 1. Spring boot 

- ###  프로젝트 폴더 구조 정의

​	- projName

​		- src

​			-main

​				- java		: java 파일 - 컴파일 시 WEB-INF로 이동

​				- resource	: 설정파일(properties, xml 등)

​				- webapp	 : 웹자원

​					- WEB-INF : 웹어플 정보파일(보안) : 외부 url을 통해 접근 불가(servlet자체에서 막는다.)

​					- META-INF : JAVA 설정파일

​			- test

​				- java

​				- resource

​		- bin

# 2. Maven 외부라이브러리 추가방법 2가지

- ##### Maven을 CLI를 통해 직접 install 하여 추가 후 pom.xml 또는 build.gradle작성 

- ##### webapp/WEB-INF/lib 위치에 jar 저장 후 pom.xml 또는 build.gradle작성 