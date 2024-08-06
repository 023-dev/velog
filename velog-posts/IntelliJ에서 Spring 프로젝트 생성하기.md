<h3 id="서론">서론</h3>
<p> IntelliJ IDEA를 활용하여 Spring MVC 프로젝트를 설정하고, Tomcat 서버를 통해 로컬에서 웹 애플리케이션을 실행하는 과정을 단계별 과정을 자세히 설명하겠다.</p>
<h3 id="프로젝트-초기화-및-설정">프로젝트 초기화 및 설정</h3>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/283110b3-595f-4db7-9a6d-348e9b906408/image.png" /></p>
<h4 id="1-maven-프로젝트-생성">1. <strong>Maven 프로젝트 생성</strong></h4>
<ul>
<li>IntelliJ IDEA에서 [New Project]를 선택하고, [Maven] 프로젝트 타입을 선택한다.</li>
<li>[Create from archetype] 체크박스를 선택하고, <code>maven-archetype-webapp</code>을 선택하여 프로젝트를 생성한다.</li>
<li>프로젝트 이름, 위치, 사용할 JDK 버전을 설정한다.</li>
</ul>
<h4 id="2-pomxml-설정">2. <strong>pom.xml 설정</strong></h4>
<ul>
<li><p>아래 의존성을 <code>pom.xml</code>에 추가하여 Spring 및 필요한 라이브러리를 포함시킨다.</p>
<pre><code class="language-pom">&lt;!-- spring  --&gt;
&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-context&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-beans&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-core&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-aop&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;!-- spring mvc --&gt;
&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-webmvc&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-web&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;javax.servlet&lt;/groupId&gt;
 &lt;artifactId&gt;javax.servlet-api&lt;/artifactId&gt;
 &lt;version&gt;3.1.0&lt;/version&gt;
 &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;javax.servlet.jsp.jstl&lt;/groupId&gt;
 &lt;artifactId&gt;jstl-api&lt;/artifactId&gt;
 &lt;version&gt;1.2&lt;/version&gt;
 &lt;type&gt;jar&lt;/type&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;javax.servlet.jsp&lt;/groupId&gt;
 &lt;artifactId&gt;jsp-api&lt;/artifactId&gt;
 &lt;version&gt;2.2&lt;/version&gt;
 &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;cglib&lt;/groupId&gt;
 &lt;artifactId&gt;cglib&lt;/artifactId&gt;
 &lt;version&gt;2.2.2&lt;/version&gt;
&lt;/dependency&gt;

&lt;!-- junit test --&gt;
&lt;dependency&gt;
 &lt;groupId&gt;junit&lt;/groupId&gt;
 &lt;artifactId&gt;junit&lt;/artifactId&gt;
 &lt;version&gt;3.8.1&lt;/version&gt;
 &lt;scope&gt;test&lt;/scope&gt;
&lt;/dependency&gt;

&lt;!-- database --&gt;
&lt;dependency&gt;
 &lt;groupId&gt;org.mybatis&lt;/groupId&gt;
 &lt;artifactId&gt;mybatis&lt;/artifactId&gt;
 &lt;version&gt;3.4.6&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.mybatis&lt;/groupId&gt;
 &lt;artifactId&gt;mybatis-spring&lt;/artifactId&gt;
 &lt;version&gt;1.3.2&lt;/version&gt;
&lt;/dependency&gt;

&lt;dependency&gt;
 &lt;groupId&gt;org.springframework&lt;/groupId&gt;
 &lt;artifactId&gt;spring-jdbc&lt;/artifactId&gt;
 &lt;version&gt;4.3.18.RELEASE&lt;/version&gt;
&lt;/dependency&gt;</code></pre>
</li>
</ul>
<h3 id="2-웹-애플리케이션-설정-및-파일-조정">2. 웹 애플리케이션 설정 및 파일 조정</h3>
<h4 id="1-webxml-파일-수정">1. <strong>web.xml 파일 수정</strong></h4>
<ul>
<li><p><code>WEB-INF</code> 폴더 아래에 위치한 <code>web.xml</code>을 열고, Spring의 <code>DispatcherServlet</code>을 설정한다.</p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;web-app xmlns=&quot;http://xmlns.jcp.org/xml/ns/javaee&quot;
    xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
    xsi:schemaLocation=&quot;http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd&quot;
    version=&quot;4.0&quot;&gt;

&lt;display-name&gt;Spring MVC&lt;/display-name&gt;

&lt;!-- Root context --&gt;
&lt;context-param&gt;
   &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
   &lt;param-value&gt;/WEB-INF/root-context.xml&lt;/param-value&gt;
&lt;/context-param&gt;

&lt;listener&gt;
   &lt;listener-class&gt;org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;
&lt;/listener&gt;

&lt;!-- servlet context --&gt;
&lt;servlet&gt;
   &lt;servlet-name&gt;appServlet&lt;/servlet-name&gt;
   &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;
   &lt;init-param&gt;
       &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
       &lt;param-value&gt;/WEB-INF/servlet-context.xml&lt;/param-value&gt;
   &lt;/init-param&gt;
   &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
&lt;/servlet&gt;
&lt;servlet-mapping&gt;
   &lt;servlet-name&gt;appServlet&lt;/servlet-name&gt;
   &lt;url-pattern&gt;/&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;

&lt;!-- encoding filter --&gt;
&lt;filter&gt;
   &lt;filter-name&gt;encodingFilter&lt;/filter-name&gt;
   &lt;filter-class&gt;
       org.springframework.web.filter.CharacterEncodingFilter
   &lt;/filter-class&gt;
   &lt;init-param&gt;
       &lt;param-name&gt;encoding&lt;/param-name&gt;
       &lt;param-value&gt;UTF-8&lt;/param-value&gt;
   &lt;/init-param&gt;
&lt;/filter&gt;
&lt;filter-mapping&gt;
   &lt;filter-name&gt;encodingFilter&lt;/filter-name&gt;
   &lt;url-pattern&gt;/*&lt;/url-pattern&gt;
&lt;/filter-mapping&gt;
</code></pre>
</li>
</ul>

```

<h4 id="2-spring-설정-파일-생성">2. <strong>Spring 설정 파일 생성</strong></h4>
<ul>
<li><p><code>XML Configuration File</code> 파일을 <code>WEB-INF</code> 폴더에 생성하고, 컨트롤러 및 뷰 리졸버 설정을 추가한다. 생성 시 일반 xml파일로 생성하면 안되고 Spring Config 파일로 생성해야 한다. 두 파일을 생성 한 후 servlet-context.xml 파일 내용만 아래 코드로 변경해준다.</p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
  xmlns:mvc=&quot;http://www.springframework.org/schema/mvc&quot;
  xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
  xmlns:context=&quot;http://www.springframework.org/schema/context&quot;
  xsi:schemaLocation=&quot;http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/mvc
   http://www.springframework.org/schema/mvc/spring-mvc.xsd
   http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd&quot;&gt;

&lt;!-- This tag registers the DefaultAnnotationHandlerMapping and
    AnnotationMethodHandlerAdapter beans that are required for Spring MVC  --&gt;
&lt;mvc:annotation-driven /&gt;
&lt;!-- This tag allows for mapping the DispatcherServlet to &quot;/&quot; --&gt;
&lt;mvc:default-servlet-handler /&gt;

&lt;!-- Process annotations on registered beans like @Autowired... --&gt;
&lt;context:annotation-config/&gt;
&lt;!-- 컴포넌트 스캔 base-package를 자신의 groupId/artifactId로 변경한다.(프로젝트 생성시 설정한 이름. 모르면 pom.xml 참고)--&gt;
&lt;context:component-scan base-package=&quot;com.example.mvcproject&quot; /&gt;

&lt;bean id=&quot;viewResolver&quot;
     class=&quot;org.springframework.web.servlet.view.InternalResourceViewResolver&quot;&gt;
   &lt;property name=&quot;prefix&quot; value=&quot;/WEB-INF/views/&quot; /&gt;
   &lt;property name=&quot;suffix&quot; value=&quot;.jsp&quot; /&gt;
&lt;/bean&gt;
</code></pre>
</li>
</ul>

```
#### 3. **Spring module, Application context 추가**
   - IntelliJ에서 [File] > [Project Structue] > [Project Setting] > [Modules] 선택 후 Spring 추가 그리고, add(+) 선택 후 위에서 생성한 xml 파일들을 선택해준다
![](https://velog.velcdn.com/images/023-dev/post/cbebbbc4-2332-40e6-97ea-51f3021ceb66/image.png)

<h4 id="4-tomcat-서버-설정">4. <strong>Tomcat 서버 설정</strong></h4>
<ul>
<li>IntelliJ에서 [Edit Configurations] &gt; [Tomcat Server] &gt; [Local]을 선택하고, Tomcat 서버의 설치 경로를 지정한다. Deployments 탭에서 애플리케이션을 추가하고, context 경로를 설정한다.</li>
</ul>
<h3 id="결론">결론</h3>
<p>위 단계를 통해 IntelliJ에서 Spring MVC 프로젝트를 성공적으로 설정하였다. </p>