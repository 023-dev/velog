<h3 id="서론">서론</h3>
<p>이 글에서는 JPA 설정에 대해 설명한다.</p>
<h3 id="1-프로젝트-세팅">1. 프로젝트 세팅</h3>
<h4 id="maven-pomxml에-hibernate-entity-database-driver-추가">Maven <code>pom.xml</code>에 hibernate-entity, database driver 추가</h4>
<pre><code class="language-xml">&lt;!-- JPA 하이버네이트 --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.hibernate&lt;/groupId&gt;
    &lt;artifactId&gt;hibernate-core&lt;/artifactId&gt;
    &lt;version&gt;6.4.2.Final&lt;/version&gt;
&lt;/dependency&gt;
&lt;!-- H2 데이터베이스 --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;com.h2database&lt;/groupId&gt;
    &lt;artifactId&gt;h2&lt;/artifactId&gt;
    &lt;version&gt;2.3.230&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;javax.xml.bind&lt;/groupId&gt;
    &lt;artifactId&gt;jaxb-api&lt;/artifactId&gt;
    &lt;version&gt;2.3.1&lt;/version&gt;
&lt;/dependency&gt;</code></pre>
<ul>
<li><p>hibernate 6.4.2.Final 버전 사용하고 JPA 표준과 하이버네이트를 포함하는 라이브러리를 추가함으로써 종속성 라이브러리도 함께 내려받는다.</p>
</li>
<li><p>데이터베이스는 자신이 사용한 데이터베이스 버전에 맞춰줘야 한다. 필자는 h2 데이터베이스의 2.3.230 버전이므로 maven에서도 일치시켰다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/df70ae47-e0ca-4f95-847a-3c0ffed460b4/image.png" /></p>
</li>
<li><p>자바 11을 사용하는 경우 java.xml.bind를 추가해주어야 한다.
자바 11부터는 해당 라이브러리가 자바 기본에서 빠져있어 이와 같은 에러 발생 가능.</p>
<pre><code class="language-java">Exception in thread &quot;main&quot; java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException</code></pre>
</li>
</ul>
<h3 id="2-jpa-설정하기">2. JPA 설정하기</h3>
<h4 id="jpa-설정-파일">JPA 설정 파일</h4>
<ul>
<li><p>파일 위치 : <code>src/main/java/resources/META_INF/persistence.xml</code></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;!-- JPA 버전 : 2.2 --&gt;
&lt;persistence version=&quot;2.2&quot;
           xmlns=&quot;http://xmlns.jcp.org/xml/ns/persistence&quot;
           xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
           xsi:schemaLocation=&quot;http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd&quot;&gt;
&lt;!-- JPA 이름 : hello --&gt;
&lt;persistence-unit name=&quot;hello&quot;&gt;
      &lt;properties&gt;
          &lt;!-- 필수 속성 : 데이터베이스 접근 정보 --&gt;
          &lt;property name=&quot;jakarta.persistence.jdbc.driver&quot; value=&quot;org.h2.Driver&quot;/&gt;
          &lt;property name=&quot;jakarta.persistence.jdbc.user&quot; value=&quot;sa&quot;/&gt;
          &lt;property name=&quot;jakarta.persistence.jdbc.password&quot; value=&quot;&quot;/&gt;
          &lt;property name=&quot;jakarta.persistence.jdbc.url&quot; value=&quot;jdbc:h2:tcp://localhost/~/test&quot;/&gt;
          &lt;property name=&quot;hibernate.dialect&quot; value=&quot;org.hibernate.dialect.H2Dialect&quot;/&gt;

          &lt;!-- 옵션 --&gt;
          &lt;property name=&quot;hibernate.show_sql&quot; value=&quot;true&quot;/&gt;
          &lt;property name=&quot;hibernate.format_sql&quot; value=&quot;true&quot;/&gt;
          &lt;property name=&quot;hibernate.use_sql_comments&quot;  value=&quot;true&quot;/&gt;
          &lt;property name=&quot;hibernate.hbm2ddl.auto&quot; value=&quot;update&quot; /&gt;
      &lt;/properties&gt;
  &lt;/persistence-unit&gt;
</code></pre>
</li>
</ul>

```
- persistence version은 JPA의 버전을 의미한다.
- pom.xml에 설정하였던 persistence-unit name 이름을 지정한다. 일반적으로 연결할 데이터베이스당 하나의 영속성 유닛을 등록한다.
- jakarta.persistence.jdbc.driver : JDBC 드라이버를 의미한다.
- jakarta.persistence.jdbc.user : 데이터베이스 접속 아이디를 의미한다.
- jakarta.persistence.jdbc.password : 데이터베이스 접속 비밀번호를 의미한다.
- jakarta.persistence.jdbc.url : 데이터베이스 접속 URL을 의미한다.
- hibernate.show_sql : 하이버네이트가 실행한 SQL을 출력한다.
- hibernate.format_sql : 하이버네이트가 실행한 SQL을 출력할 때 보기 쉽게 정렬한다.
- hibernate.use_sql_comments : 쿼리를 출력할 때 주석도 함께 출력한다.
- hibernate.hbm2ddl.auto : 애플리케이션 실행 시 데이터베이스 스키마를 자동으로 생성하거나 업데이트하는 방법을 지정한다.
#### 데이터베이스 방언
- JPA는 특정 데이터베이스에 종속되지 않는다.
- 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다르다.
- 방언 : SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능
- 하이버네이트는 40가지 이상의 데이터베이스 방언을 지원한다.
```xml
<!-- H2 -->

<!-- Oracle -->

<!-- MySQL -->

```
![](https://velog.velcdn.com/images/023-dev/post/7ad1298f-3ece-4332-9359-2a80863e4f5d/image.png)

<h3 id="3-jpa-구동-방식">3. JPA 구동 방식</h3>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/9bfe7277-e57f-4504-9cc6-687815f91f30/image.png" /></p>
<pre><code class="language-java">package hellojpa;

import jakarta.persistence.*;
public class JpaMain {

    public static void main(String[] args) {
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory(&quot;hello&quot;);

        EntityManager entityManager = entityManagerFactory.createEntityManager();

        EntityTransaction entityTransaction = entityManager.getTransaction();
        entityTransaction.begin();

        try {
            Member member = new Member();
            member.setId(1L);
            member.setName(&quot;A&quot;);
            entityManager.persist(member);
            entityTransaction.commit();
        } catch (Exception e) {
            entityTransaction.rollback();
        } finally {
            entityManager.close();
        }
        entityManagerFactory.close();
    }
}
</code></pre>
<ul>
<li><p>EntityMangerFactory 생성</p>
<pre><code class="language-java">EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory(&quot;hello&quot;);</code></pre>
<ul>
<li>JPA를 시작하려면 우선 persistence.xml의 설정 정보를 사용해서 엔티티 매니저 팩토리를 생성해야 한다. </li>
<li><code>META-INF/persistence.xml</code>에서 이름이 jpabook인 영속성 유닛(persistence-unit)을 찾아서 엔티티 매니저 팩토리를 생성한다. </li>
<li>엔티티 매니저 팩토리는 애플리케이션 전체에서 딱 한 번만 생성하고 공유해서 사용해야 한다.</li>
</ul>
</li>
<li><p>EntityManger 생성</p>
<pre><code class="language-java">EntityManager entityManager = emf.createEntityManager();</code></pre>
<ul>
<li>JPA의 기능 대부분은 엔티티 매니저가 제공한다.</li>
<li>엔티티 매니저를 사용해서 엔티티를 데이터베이스에 등록/수정/삭제/조회할 수 있다.</li>
<li>엔티티 매니저는 내부에 데이터베이스 커넥션을 유지하면서 데이터베이스와 통신한다.</li>
<li>엔티티 매니저는 데이터베이스 커넥션과 밀접한 관계가 있으므로 쓰레드 간에 공유하거나 재사용하면 안 된다.</li>
</ul>
</li>
<li><p>EntityTransaction 관리</p>
<pre><code class="language-java">EntityTransaction tx = em.getTransaction();
tx.begin();
tx.commit();</code></pre>
<ul>
<li>JPA를 사용하면 항상 트랜잭션 안에서 데이터를 변경해야 한다.</li>
<li>트랜잭션 없이 데이터를 변경하면 예외가 발생한다.</li>
<li>엔티티 매니저에서 트랜잭션 API를 받아와야 한다.</li>
</ul>
</li>
<li><p>종료</p>
<pre><code class="language-java">em.close();
emf.close();</code></pre>
<ul>
<li>사용이 끝난 엔티티 매니저는 반드시 종료해야 한다.</li>
<li>애플리케이션을 종료할 때 엔티티 매니저 팩토리도 종료해야 한다.<h3 id="4-jpa를-이용한-crud">4. JPA를 이용한 CRUD</h3>
등록, 수정, 삭제, 조회 작업은 엔티티 매니저를 통해서 수행된다.</li>
</ul>
</li>
</ul>
<pre><code class="language-java">//객체 생성
Member member = new Member();
member.setId(1L);
member.setName(&quot;A&quot;);

//Create
em.persist(member);

//한 건 조회, Read
Member findMember = em.find(Member.class, 1L);
System.out.println(findMember.getId()); // 1
System.out.println(findMember.getName()); // &quot;A&quot;

//Update
Member updateMember = em.find(Member.class, 1L);
updateMember.setName(&quot;B&quot;);

//Delete
Member deleteMember = em.find(Member.class, 1L);
em.remove(deleteMember);

//목록 조회, Read (JPQL)
List&lt;Member&gt; result = em.createQuery(&quot;select m from Member as m&quot;, Member.class).getResultList();
for (Member mem : result) {
System.out.println(&quot;member.getName() = &quot; + mem.getName());
}</code></pre>
<ul>
<li>Create<pre><code class="language-java">em.persist(member);</code></pre>
<ul>
<li>엔티티를 저장하려면 엔티티 매니저의 persist() 메서드에 저장할 엔티티를 넘겨주면 된다.</li>
</ul>
</li>
<li>Read<pre><code class="language-java">//1개 조회
Member findMember = em.find(Member.class, 1L);
//목록 조회 (JPQL)
List&lt;Member&gt; result = em.createQuery(&quot;select m from Member as m&quot;, Member.class) .getResultList();</code></pre>
<ul>
<li>find()를 이용하면 SELECT SQL을 생성하고 실행할 수 있다.</li>
<li>필요한 데이터만 데이터베이스에서 불러오려면 결구 검색 조건이 포함된 SQL을 사용해야 한다. 이런 경우JPA에서 제공하는 JPQL(Java Persistence Query Language)를 사용한다.</li>
</ul>
</li>
<li>Update<pre><code class="language-java">updateMember.setName(&quot;helloJPA&quot;);</code></pre>
<ul>
<li>단순히 엔티티의 값만 변경하면 JPA가 어떤 엔티티가 변경되었는지 추적하여서 UPDATE SQL을 생성하고 실행된다.</li>
</ul>
</li>
<li>Delete<pre><code class="language-java">em.remove(deleteMember);</code></pre>
<ul>
<li>DELETE SQL이 생성하고 실행된다.</li>
</ul>
</li>
<li>SQL vs JPQL<ul>
<li>SQL은 데이터베이스 테이블을 대상으로 쿼리한다.</li>
<li>JPQL은 엔티티 객체를 대상으로 쿼리한다.</li>
<li>from Member는 회원 엔티티 객체를 뜻한다. JPQL은 데이터베이스 테이블을 전혀 알지 못한다.</li>
</ul>
</li>
</ul>