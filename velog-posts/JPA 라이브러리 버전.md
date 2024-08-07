<p>이 글에서는 JPA 라이브러리의 버전을 어떻게 선택하는 지 설명한다.</p>
<ol>
<li><a href="https://spring.io/">Spring</a>로 이동한다.</li>
<li><code>Projects</code>를 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/75e80ab6-d358-443f-99f3-5d4f0cf7d0d6/image.jpg" /></li>
<li><code>Spring Boot</code>를 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/f489e54c-d43d-4070-a38a-79c57b5f2755/image.jpg" /></li>
<li><code>LEARN</code>을 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/ee902304-e743-4dfe-a097-7dd923b74254/image.jpg" /></li>
<li>사용할 Spring Boot의 Version의 <code>Reference Doc.</code> 을 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/0f1d2aa3-92c1-43a8-ae84-6ad812ab3464/image.jpg" /></li>
<li><code>a single HTML page</code>를 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/3ea40b99-4a8b-4471-af8b-6c6e42b51357/image.jpg" /></li>
<li><code>⌘+F</code>를 눌러 <code>org.hibernate.orm</code>를 입력한다. 
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/231ed215-90b2-492d-ad69-4d436acdad13/image.jpg" /></li>
<li><code>Managed Dependency Coordinates</code>에 있는 <code>Version</code> 정보를 찾는다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/9c6a7dd1-1a9d-4d0d-98cd-e17396e25fd7/image.jpg" /></li>
<li>[Maven Repository]로 이동한다.(<a href="https://mvnrepository.com/">https://mvnrepository.com/</a>)</li>
<li><code>Search</code>에 <code>org.hibernate.orm</code>을 입력하여 검색한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/47b18b9a-5b6c-4ac9-ace0-767a50621c25/image.jpg" /></li>
<li><code>Hibernate ORM Hibernate Core</code>를 찾아 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/490846af-44bd-45c3-95a9-a5e59e9fc428/image.jpg" /></li>
<li><code>8번</code>에서 찾은 Version을 찾아 클릭한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/5d9f002b-c9d5-44fd-80a2-7262d9f5623b/image.jpg" /></li>
<li>자신의 빌드 유형에 맞는 텍스트를 복사한다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/6e981984-fa5e-45bf-9ff8-2e4ab8ca8798/image.jpg" /></li>
</ol>
<p>14.필자의 환경은 Maven이므로 Maven Xml을 복사해서 <code>pom.xml</code>에 추가했다.</p>
<pre><code class="language-xml">&lt;!-- https://mvnrepository.com/artifact/org.hibernate.orm/hibernate-core --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.hibernate.orm&lt;/groupId&gt;
    &lt;artifactId&gt;hibernate-core&lt;/artifactId&gt;
    &lt;version&gt;6.4.9.Final&lt;/version&gt;
&lt;/dependency&gt;
</code></pre>