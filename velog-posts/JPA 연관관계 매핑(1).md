<h3 id="서론">서론</h3>
<p>이 글에서는 JPA 연관관계 매핑(Relationship Mapping)에 대해 설명한다.</p>
<h3 id="1-단방향-연관관계">1. 단방향 연관관계</h3>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/b3df2067-3a5f-4f52-bd1f-4e1bc3e4ce4c/image.png" /></p>
<ul>
<li><p>객체 연관관계에서 회원 객체는 Member.team 멤버 변수를 통해 팀 객체와 연관관계를 맺는다. 이 관계는 단방향 관계로, 회원 객체를 통해 팀 객체를 알 수 있지만, 팀 객체를 통해 회원 객체를 알 수는 없다. 즉, member.getTeam()은 가능하지만, team.getMember()는 불가능하다.</p>
</li>
<li><p>테이블 연관관계에서 회원 테이블은 TEAM_ID 외래 키로 팀 테이블과 연관관계를 맺는다. 이 관계는 양방향 관계로, 회원 테이블의 TEAM_ID 외래 키를 통해 회원과 팀을 조인할 수 있으며, 반대로 팀과 회원도 조인할 수 있다.</p>
</li>
<li><p>객체 연관관계와 테이블 연관관계의 가장 큰 차이는 참조를 통한 연관관계는 언제나 단방향이라는 점이다. 객체 연관관계에서 양쪽에서 서로 참조하는 것을 양방향 연관관계라 하지만, 정확히 이야기하면 이는 양방향 관계가 아니라 서로 다른 단방향 관계 2개로 이루어진 것이다.</p>
<h4 id="11-객체-관계-매핑">1.1. 객체 관계 매핑</h4>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/da57a06a-8ed8-483d-ace0-5243f3191a94/image.png" /></p>
<pre><code class="language-java">@Entity
@Getter @Setter
public class Member {
    @Id @GeneratedValue
    @Column(name = &quot;MEMBER_ID&quot;)
    private Long id;

    @Column(name = &quot;USERNAME&quot;)
    private String name;

    @ManyToOne
    @JoinColumn(name = &quot;TEAM_ID&quot;)
    private Team team;
}
@Entity
@Getter @Setter
public class Team {
    @Id @GeneratedValue
    @Column(name = &quot;TEAM_ID&quot;)
    private Long id;

    private String name;
}</code></pre>
<ul>
<li><p>@ManyToOne</p>
<ul>
<li><p>다대일(N:1) 관계를 나타내는 매핑 정보이다. 연관관계를 매핑할 때 이러한 다중성을 나타내는 어노테이션은 필수적으로 사용해야 한다.</p>
<table>
<thead>
<tr>
<th align="center">속성</th>
<th align="center">기능</th>
<th align="center">기본값</th>
</tr>
</thead>
<tbody><tr>
<td align="center">optional</td>
<td align="center">false로 설정하면 연관된 연관된 엔티티가 항상 있어야 한다.</td>
<td align="center">true</td>
</tr>
<tr>
<td align="center">fetch</td>
<td align="center">글로벌 페치 전략을 설정한다.</td>
<td align="center">@ManyToOne=FetchType.EAGER<br />@OneToMany=FetchType.LAZY</td>
</tr>
<tr>
<td align="center">cascade</td>
<td align="center">영속성 전이 기능을 사용한다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">targetEntity</td>
<td align="center">연관된 엔티티의 타입 정보를 설정한다. 이 기능은 거의 사용하지 않는다. 컬렉션을 사용해도 제네릭으로 타입 정보를 알 수 있다.<br />ex)@OneToMany<br />private List members<br />@OneToMany(targetEntity=Member.class)<br />private List members;</td>
<td align="center"></td>
</tr>
</tbody></table>
</li>
</ul>
</li>
</ul>
</li>
<li><p>@JoinColumn</p>
<ul>
<li><p>외래 키를 매핑할 때 사용된다. name 속성에는 매핑할 외래 키 이름을 지정한다.</p>
<table>
<thead>
<tr>
<th align="center">속성</th>
<th align="center">기능</th>
<th align="center">기본값</th>
</tr>
</thead>
<tbody><tr>
<td align="center">name</td>
<td align="center">매핑할 외래 키 이름</td>
<td align="center">필드명 +_+ 참조하는 테이블의 기본 키 컬럼명<br />ex) 필드명(team),<br />참조하는 데이블의 컬럼명(TEAM_ID)<br />=&gt; team_TEAM_ID</td>
</tr>
<tr>
<td align="center">referencedColumnName</td>
<td align="center">외래 키가 참조하는 대상 테이블의 컬럼명</td>
<td align="center">참조하는 테이블의 기본키 컬럼명</td>
</tr>
<tr>
<td align="center">foriegnKey(DDL)</td>
<td align="center">외래 키 제약조건을 직접 지정할 수 있다. 이 속성은 테이블을 생성할 때만 사용한다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">unique<br />nullable<br />insertable<br />updateable<br />columnDefinition<br />table</td>
<td align="center">@Column의 속성과 같다.</td>
<td align="center"></td>
</tr>
</tbody></table>
</li>
</ul>
</li>
</ul>
<h4 id="12-연관관계-등록-조회">1.2. 연관관계 등록, 조회</h4>
<pre><code class="language-java">//단방향 연관관계
Team team = new Team();
team.setName(&quot;TeamA&quot;);
em.persist(team);

Member member = new Member();
member.setName(&quot;member1&quot;);
member.setTeam(team);
em.persist(member);

em.flush(); //insert 쿼리가 나간다.
em.clear(); 
//영속성 context를 초기화 해주었기 때문에 뒤에 나오는 em.find() 메소드를 통해 select 쿼리가 나간다.

Member findMember = em.find(Member.class, member.getId());
Team findTeam = findMember.getTeam(); //객체 그래프 탐색
System.out.println(findTeam.getName());</code></pre>
<h3 id="2-양방향-연관관계">2. 양방향 연관관계</h3>
<p><img alt="" src="https://velog.velcdn.com/images/023-dev/post/fd0f7d40-2c60-4602-8dd6-b6814dd3f133/image.png" /></p>
<ul>
<li>객체 연관관계<ul>
<li>팀에서 회원은 일대다 관계이다.</li>
<li>일대다 관계는 여러 건과 연관관계를 맺을 수 있으므로 컬렉션을 사용해야 한다.</li>
<li>List 컬렉션 추가</li>
</ul>
</li>
<li>데이블 관계<ul>
<li>데이터베이스 테이블은 외래 키 하나의 양방향으로 조회할 수 있다.</li>
<li>데이터베이스에 추가할 내용은 전혀 없다.</li>
</ul>
</li>
</ul>
<p>2.1. 객체 관계 매핑</p>
<pre><code class="language-java">@Entity
@Getter @Setter
public class Team {
    @Id @GeneratedValue
    @Column(name = &quot;TEAM_ID&quot;)
    private Long id;

    private String name;

    @OneToMany(mappedBy = &quot;team&quot;) //Member class의 team 변수와 연관관계 매핑되어있다.
    private List&lt;Member&gt; members = new ArrayList&lt;&gt;();
}</code></pre>
<ul>
<li>Member 엔티티는 변경한 부분이 없다. Team 엔티티에는 List members를 추가하고, @OneToMany 매핑 정보를 사용한다. mappedBy 속성은 양방향 매핑일 때 사용하는데, 반대쪽 매핑의 필드 이름을 값으로 지정하면 된다.</li>
</ul>
<p>2.2. 연관관계 등록, 조회</p>
<pre><code class="language-java">//양방향 연관관계
Team team = new Team();
team.setName(&quot;TeamA&quot;);
em.persist(team);

Member member = new Member();
member.setName(&quot;member1&quot;);
member.setTeam(team);
em.persist(member);

em.flush();
em.clear();

Member findMember = em.find(Member.class, member.getId());
List&lt;Member&gt; members = findMember.getTeam().getMembers(); //객체 그래프 탐색
for (Member m : members) {
    System.out.println(m.getName());
}  </code></pre>
<ul>
<li>회원 컬렉션으로 객체 그래프 탐색을 사용할 수 있다.</li>
</ul>
<h3 id="3-연관관계-주인">3. 연관관계 주인</h3>
<p>3.1. 연관관계 주인의 필요성</p>
<ul>
<li>단순히 @OneToMany만 있으면 되지 mappedBy는 왜 필요할까? 객체 연관관계는 두 개로 나눌 수 있다 
<code>회원 -&gt; 팀 연관관계 (단방향)</code>
<code>팀 -&gt; 회원 연관관계 (단방향)</code>
그러나 테이블 연관관계는 하나로, <code>회원 &lt;-&gt; 팀</code>의 연관관계로 표현된다. 엔티티를 양방향 연관관계로 설정하면 객체의 참조는 두 개지만, 외래 키는 하나만 존재하여 차이가 발생한다. 따라서 두 객체 연관관계 중 하나를 정해서 테이블의 외래 키를 관리해야 하는데, 이를 연관관계의 주인이라고 한다.</li>
</ul>
<p>3.2. 연관관계 주인</p>
<ul>
<li>연관관계 주인 개념에서는 두 연관관계 중 하나를 연관관계의 주인으로 정해야 하며, 연관관계의 주인만이 데이터베이스 연관관계와 매핑되고 외래 키를 관리(등록, 수정, 삭제)할 수 있다. 주인이 아닌 쪽은 <code>mappedBy</code> 속성을 사용하여 연관관계의 주인을 지정해야 한다.</li>
</ul>
<p>3.3. 연관관계 주인 정하기</p>
<ul>
<li>연관관계의 주인을 정하는 것은 사실 외래 키 관리자를 선택하는 것이다. 만약 회원 엔티티에 있는 Member.team을 주인으로 선택하면 자기 테이블에 있는 외래 키를 관리하게 된다. 하지만 팀 엔티티에 있는 Team.members를 주인으로 선택하면 물리적으로 전혀 다른 테이블의 외래 키를 관리해야 한다. 따라서 연관관계의 주인은 외래 키가 있는 곳으로 정한다. 연관관계의 주인만 데이터베이스 연관관계와 매핑되고 외래 키를 관리할 수 있으며, 주인이 아닌 반대편은 읽기만 가능하고 외래 키를 변경할 수 없다.
<img alt="" src="https://velog.velcdn.com/images/023-dev/post/5e488ced-3f3a-4a32-81cd-2c5d5ad0fbca/image.png" /><h3 id="4-양방향-연관관계의-주의점">4. 양방향 연관관계의 주의점</h3>
</li>
</ul>
<p>4.1. 역방향(주인이 아닌 방향)만 값을 입력</p>
<pre><code class="language-java">// 양방향 연관관계와 연관관계 주인 - 주의
Member member = new Member();
member.setName(&quot;member1&quot;);
em.persist(member);

Team team = new Team();
team.setName(&quot;TeamA&quot;);
team.getMembers().add(member); //읽기 전용이다. 값을 넣어줘도 DB에 반영되지 않는다.
em.persist(team);</code></pre>
<ul>
<li>데이터베이스에서 MEMBER 테이블을 조회하게 되면 TEAM_ID가 null 값으로 입력되어 있다. 연관관계의 주인이 아닌 쪽에서 값을 수정해도 DB에 반영되지 않으며, 연관관계의 주인만이 외래 키의 값을 변경할 수 있다.</li>
</ul>
<p>4.2. 순수한 객체까지 고려한 양방향 연관관계</p>
<ul>
<li><p>연관관계의 주인에만 값을 저장하고 주인이 아닌 곳에는 값을 저장하지 않아도 될까? 객체 관점에서는 양쪽 방향에 모두 값을 입력해주는 것이 가장 안전하다. 이는 JPA를 이용하지 않고 Test 케이스를 작성하는 경우에도 필요하다. 객체까지 고려하여 주인이 아닌 곳에도 값을 입력하는 것이 좋으며, 객체의 양방향 연관관계는 양쪽 모두 관계를 맺어주는 것이 바람직하다.</p>
<pre><code class="language-java">// 양방향 연관관계와 연관관계 주인
Team team = new Team();
team.setName(&quot;TeamA&quot;);
em.persist(team);

Member member = new Member();
member.setName(&quot;member1&quot;);

member.setTeam(team); //연관관계의 주인
team.getMembers().add(member); //주인이 아니다. 저장 시 사용되지 않는다.
em.persist(member);</code></pre>
</li>
</ul>
<p>(3) 연관관계 편의 메소드</p>
<ul>
<li><p>양방향 연관관계는 양쪽 모두 신경 써야 한다. 호출하다 보면 실수로 둘 중 하나만 호출하여 양방향 관계가 깨질 수 있다. 양방향 관계에서 두 코드를 하나인 것처럼 사용하는 것이 안전하다. 예를 들어, changeTeam 메소드 하나로 양방향 관계를 모두 설정하도록 변경할 수 있다. 이렇게 한 번에 양방향 관계를 설정하는 메소드를 연관관계 편의 메소드라고 한다.</p>
<pre><code class="language-java">@Entity
@Getter @Setter
public class Member {
    ...
    @ManyToOne
    @JoinColumn(name = &quot;TEAM_ID&quot;)
    private Team team;

    public void changeTeam(Team team) {
        this.team = team;
        team.getMembers().add(this);
    }
}</code></pre>
</li>
</ul>
<ol start="5">
<li>정리</li>
</ol>
<ul>
<li>단방향 매핑만으로 테이블과 객체의 연관관계 매핑은 이미 완료되었다.<ul>
<li>우선 단방향 매핑만 하고 양방향은 필요할 때 추가해도 된다. (테이블에 영향을 주지 않는다.)</li>
<li>객체 그래프 탐색 기능(JPQL 쿼리 탐색)이 필요할 때 양방향 기능을 추가하자.</li>
</ul>
</li>
<li>연관관계의 주인은 항상 외래 키가 있는 곳으로 한다.</li>
<li>양방향 연관관계를 매핑하려면 객체에서 양쪽 방향을 모두 관리해야 한다.</li>
</ul>