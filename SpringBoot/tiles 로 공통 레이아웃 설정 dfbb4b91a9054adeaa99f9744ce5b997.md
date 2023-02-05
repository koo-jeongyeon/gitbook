# tiles 로 공통 레이아웃 설정

생성일: 2021년 2월 16일 오후 2:46

pom.xml

```xml
<!-- 타일즈적용 -->
			<dependency>
			<groupId>org.apache.tiles</groupId>
			<artifactId>tiles-extras</artifactId>
			<version>3.0.5</version>
		</dependency>
		<dependency>
			<groupId>org.apache.tiles</groupId>
			<artifactId>tiles-servlet</artifactId>
			<version>3.0.5</version>
		</dependency>
		<dependency>
			<groupId>org.apache.tiles</groupId>
			<artifactId>tiles-jsp</artifactId>
			<version>3.0.5</version>
		</dependency>

```

서블릿 컨텍스트

```xml
<!-- Tiles 뷰 리졸버 -->
	<beans:bean id="tielsViewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
	    <beans:property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView" />
	    <beans:property name="order" value="1" />
	</beans:bean>
	<!-- Tiles 설정 파일 -->
	<beans:bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
	    <beans:property name="definitions">
	        <beans:list>
	            <beans:value>/WEB-INF/tiles/tiles.xml</beans:value>
	        </beans:list>
	    </beans:property>
	</beans:bean>
```

tiles.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>

    <!-- 기본 레이아웃 -->
    <definition name="layout" template="/WEB-INF/tiles/components/layout.jsp">
        <put-attribute name="header" value="/WEB-INF/tiles/components/header.jsp" />
        <put-attribute name="content" value="" />
        <put-attribute name="footer" value="/WEB-INF/tiles/components/footer.jsp" />
    </definition>
    
    <definition name="layout/*/*" extends="layout">
        <put-attribute name="content" value="/WEB-INF/jsp/{1}/{2}.jsp" />
    </definition>

    <!-- 레이아웃 없음 -->
    <definition name="notLayout" template="/WEB-INF/tiles/components/layout.jsp">
        <put-attribute name="header" value="" />
        <put-attribute name="content" value="" />
        <put-attribute name="footer" value="" />
    </definition>
    
    <definition name="notLayout/*/*" extends="notLayout">
        <put-attribute name="content" value="/WEB-INF/jsp/{1}/{2}.jsp" />
    </definition>

    <!-- 리딩스쿨 레이아웃 -->
    <definition name="schoollayout" template="/WEB-INF/tiles/components/schoollayout.jsp">
    	<put-attribute name="header" value="" />
        <put-attribute name="content" value="" />
        <put-attribute name="footer" value="" />
    </definition>
    
    <definition name="schoollayout/*/*" extends="schoollayout">
        <put-attribute name="content" value="/WEB-INF/jsp/{1}/{2}.jsp" />
    </definition>

    <!-- 멤버 레이아웃 -->
    <definition name="memberLayout" template="/WEB-INF/tiles/components/layout.jsp">
    	<put-attribute name="header" value="/WEB-INF/tiles/components/header_member.jsp" />
        <put-attribute name="content" value="" />
        <put-attribute name="footer" value="/WEB-INF/tiles/components/footer_member.jsp" />
    </definition>
    
    <definition name="memberLayout/*/*" extends="memberLayout">
        <put-attribute name="content" value="/WEB-INF/jsp/{1}/{2}.jsp" />
    </definition>
</tiles-definitions>
```

이런식으로 레이아웃 만들어주면됨