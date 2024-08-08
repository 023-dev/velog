<h3 id="서론">서론</h3>
<p> 이 글에서는 JPA의 영속성 컨텍스트(Persistence Context)에 대해 설명한다.</p>
<h3 id="1-영속성-컨텍스트">1. 영속성 컨텍스트</h3>
<ul>
<li>엔티티를 영구 저장하는 논리적인 환경으로 엔티티 매니저(Entity Manager)로 엔티티를 저장(persist)하거나 조회하면 영속성 컨텍스트에 저장한다.</li>
<li>영속성 컨텍스트는 엔티티 매니저를 통해서 접근하고 관리하고 보관할 수 있다.</li>
<li>영속성 컨텍스트는 엔티티 매니저를 생성할 때마다 1:1로 생성되지만 스프링환경이라면 M:1으로 하나의 영속성 컨텍스트에 여러 개의 엔티티 매니저를 붙을 수 있다. 
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/ee3222bd-bd3c-44ff-8c91-e9ccb9317ff7/image.png" /></li>
</ul>
<h3 id="2-엔티티의-생명-주기">2. 엔티티의 생명 주기</h3>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/1ff05643-21b9-469c-a809-1000f4a9aef4/image.png" /></p>
<ul>
<li><h4 id="비영속-newtransient--영속성-컨텍스트와-전혀-관계가-없는-새로운-상태">비영속 (new/transient) : 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태</h4>
<pre><code class="language-java">//객체를 생성한 상태(비영속)
Member member = new Member();
member.setId(&quot;member1&quot;);
member.setUsername(&quot;회원1&quot;);</code></pre>
<ul>
<li>엔티티 객체를 생성한 상태로 순수한 객체 상태이며, 아직 영속성 컨텍스트나 데이터베이스와는 전혀 무관한 상태이다.</li>
</ul>
</li>
<li><h4 id="영속-managed--영속성-컨텍스트에-관리되는-상태">영속 (managed) : 영속성 컨텍스트에 관리되는 상태</h4>
<pre><code class="language-java">//객체를 생성한 상태(비영속)
Member member = new Member();
member.setId(&quot;member1&quot;);
member.setUsername(“회원1”);
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
//객체를 저장한 상태(영속)
em.persist(member);</code></pre>
<ul>
<li>엔티티 매니저를 통해서 엔티티를 영속성 컨텍스트에 저장 및 관리하는 상태로, 조회나 저장 또는 JPQL을 사용해서 조회한 엔티티도 영속성 컨텍스트가 관리하는 영속 상태로 들어서게 된다.</li>
</ul>
</li>
<li><h4 id="준영속-detached--영속성-컨텍스트에-저장되었다가-분리된-상태">준영속 (detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태</h4>
<pre><code class="language-java">//회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member); 
em.clear(); // 영속성 컨텍스트 초기화
em.close(); // 영속성 컨텍스트 닫기</code></pre>
<ul>
<li>영속 상태의 엔티티의 엔티티 매니저로 detach, claer, close를 핸들링하면 해당 엔티티는 준영속 상태로 되며, 영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.</li>
</ul>
</li>
<li><h4 id="삭제-removed--삭제된-상태">삭제 (removed) : 삭제된 상태</h4>
<pre><code class="language-java">em.remove(member); // 객체 삭제</code></pre>
<ul>
<li>엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.<h3 id="3-영속성-컨텍스트의-이점">3. 영속성 컨텍스트의 이점</h3>
</li>
</ul>
</li>
<li><h4 id="1차-캐시">1차 캐시</h4>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/c490c76b-0a00-40c1-af2e-dc90164c2047/image.png" /></p>
<pre><code class="language-java">//비영속 상태
Member member = new Member();
member.setId(1L);
member.setName(&quot;MemberA&quot;);
//영속 상태
em.persist(member);</code></pre>
<p>영속성 컨텍스트는 내부에 1차 캐시를 보유하고 있으며, 영속 상태의 엔티티는 모두 이곳에 저장된다. 이는 내부에 Map 구조로 구현되어 있다고 생각할 수 있다. 이 Map에서 Key는 @Id로 매핑된 식별자(즉, 데이터베이스의 기본키)이고, Value는 엔티티 인스턴스다. 따라서 영속성 컨텍스트에 데이터를 저장하고 조회하는 모든 기준은 데이터베이스의 기본 키 값에 따라 이루어진다. EntityManager의 find 메서드를 예로 들면 다음과 같이 정의된다. </p>
<pre><code class="language-java">public &lt;T&gt; T find(Class&lt;T&gt; entityClass, Object primaryKey);</code></pre>
<p>이 메서드를 사용하여 특정 엔티티 클래스타입과 기본 키 값을 통해 영속성 컨텍스트에서 데이터를 조회할 수 있다.</p>
<ul>
<li><p>1차 캐시에서 조회
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/9e142f93-fe0e-400d-a283-18d0f4e5b32d/image.png" /></p>
<pre><code class="language-java">//1차 캐시에서 조회
Member member = new Member();
member.setId(1L);
member.setName(&quot;MemberA&quot;);

//1차 캐시에 저장된다.
em.persist(member);
//1차 캐시에서 조회
Member findMember = em.find(Member.class, 1L);</code></pre>
<p>em.find()를 호출하면 우선적으로 1차 캐시에서 식별자 값으로 엔티티를 찾는다. 찾는 엔티티가 있으면 데이터베이스를 조회하지 않고 메모리에 있는 1차 캐시에서 엔티티를 조회한다.</p>
</li>
<li><p>데이터베이스에서 조회
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/a54ac905-5c1e-48dd-8137-7f0d4451f690/image.png" /></p>
<pre><code class="language-java">//데이터베이스에서 조회
Member findMember2 = em.find(Member.class, 2L);</code></pre>
<p>em.find()를 호출했는데 엔티티가 1차 캐시에 없으면 엔티티 매니저는 데이터베이스를 조회해서 엔티티를 생성한다. 그리고 1차 캐시에 저장한 후 다음 조회 때 영속상태의 엔티티를 반환한다.</p>
</li>
</ul>
</li>
<li><h4 id="동일성identity-보장">동일성(identity) 보장</h4>
<p>1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭
션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공한다.</p>
<pre><code class="language-java">//영속 엔티티의 동일성 보장
Member findMember1 = em.find(Member.class, 101L);
Member findMember2 = em.find(Member.class, 101l);
System.out.println(findMember2==findMember1);  //true</code></pre>
<p>em.find()를 반복해서 호출하는 경우 영속성 컨텍스트는 1차 캐시에 있는 같은 엔티티 인스턴스를 반환한다. 따라서 둘은 같은 인스턴스이므로 true이다. 이렇듯 영속성 컨텍스트는 엔티티의 동일성을 보장한다. </p>
</li>
</ul>
<ul>
<li><h4 id="트랜잭션을-지원하는-쓰기-지연transactional-write-behind">트랜잭션을 지원하는 쓰기 지연(transactional write-behind)</h4>
<pre><code class="language-java">EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
//엔티티 매니저는 데이터 변경 시 트랜잭션을 시작해야 한다.
tx.begin(); //트랜잭션 시작

Member member1 = new Member(150L, &quot;A&quot;);
Member member2 = new Member(160L, &quot;B&quot;);

em.persist(member1);
em.persist(member2);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

tx.commit(); //커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.</code></pre>
<table>
<thead>
<tr>
<th align="center">em.persist(memberA)</th>
<th align="center">em.persist(memberB)</th>
<th align="center">tx.commit()</th>
</tr>
</thead>
<tbody><tr>
<td align="center"></td>
<td align="center"></td>
<td align="center"></td>
</tr>
</tbody></table>
<p>엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 저장하지 않고, 내부 쓰기 지연 SQL 저장소에 INSERT SQL을 모아둔다. 그리고 트랜잭션을 커밋하면 엔티티 매니저는 영속성 컨텍스트를 플러시(flush)한다. 여기서 플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화하는 작업을 말하며, 이를 통해 등록, 수정, 삭제한 엔티티를 데이터베이스에 반영한다.</p>
</li>
<li><h4 id="변경-감지dirty-checking">변경 감지(Dirty Checking)</h4>
<pre><code class="language-java">EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

Member member = em.find(Member.class, &quot;memberA&quot;);

//영속 엔티티 데이터 수정
member.setName(&quot;JPA&quot;);

tx.commit();</code></pre>
<p>JPA로 엔티티를 수정할 때는 단순히 엔티티를 조회해서 데이터만 변경하면 된다.
트랜잭션 커밋 직전에 주석으로 처리된 em.update(member) 메서드를 실행해야 할 것 같지만 이런 메서드는 없다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/e8ec6fdc-4974-43d0-8bb6-d4d45f282bc8/image.png" />
트랜잭션을 커밋하면 엔티티 매니저 내부에서 먼저 플러시가 호출된다.
엔티티와 스냅샷을 비교해서 변경된 엔티티를 찾는다.
변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.
쓰기 지연 저장소의 SQL을 데이터베이스에 보낸다.
데이터베이스 트랜잭션을 커밋한다.</p>
</li>
<li><h4 id="지연-로딩lazy-loading">지연 로딩(Lazy Loading)</h4>
<h3 id="4-플러시flush">4. 플러시(Flush)</h3>
<ul>
<li>영속성 컨텍스트를 비우지 않고 변경 내용을 데이터베이스와 동기화하기 위해 반영하는 작업을 한다.</li>
<li>플러시 실행 시 변경 감지가 동작해서 영속성 컨텍스트에 있는 모든 엔티티를 스냅샷과 비교해서 수정된 엔티티를 찾는다. 그 후, 수정된 엔티티는 수정 쿼리를 만들어 쓰기 지연 SQL 저장소에 등록하고 저장된 쿼리를 데이터베이스에 전송한다.</li>
<li>영속성 컨텍스를 플러시하는 방법은 다음과 같다.<ul>
<li>em.flush() 직접 호출</li>
<li>트랜잭션 커밋 시 플러시 자동 호출</li>
<li>JPQL 쿼리 실행 시 플러시 자동 호출</li>
</ul>
</li>
<li>식별자를 기준으로 조회하는 find() 메서드를 호출할 때는 플러시가 실행되지 않는다.<pre><code class="language-java">Member member1 = new Member(300L, &quot;JPA&quot;);
em.persist(member1);
Member member = em.find(Member.class, 1L);
System.out.println(&quot;================&quot;);
tx.commit();</code></pre>
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/6d792e2c-08c9-478e-ac63-47fb8db66ef3/image.png" />
em.find() 호출 시에 select query는 즉시 나간다. 하지만 이전에 영속성 컨텍스트에 등록되어 있던 member1은 플러시 되지 않는다. 이후 커밋할 때 데이터베이스에 반영된다.</li>
</ul>
</li>
</ul>