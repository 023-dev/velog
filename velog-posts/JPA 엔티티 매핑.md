<h3 id="서론">서론</h3>
<p>이 글에서는 JPA의 엔티티 매핑(Entity Mapping)에 대해 설명한다.</p>
<h3 id="1-객체와-테이블-매핑">1. 객체와 테이블 매핑</h3>
<h4 id="11-entity">1.1. @Entity</h4>
<ul>
<li><p>JPA를 사용하여 테이블과 매핑할 클래스는 반드시 @Entity 어노테이션을 붙여야 하며, @Entity가 붙은 클래스는 JPA가 관리하는 엔티티라고 부른다. @Entity를 적용할 때 주의사항으로는, 파라미터가 없는 public 또는 protected 기본 생성자가 필수이며, final 클래스, enum, interface, inner 클래스에는 @Entity를 사용할 수 없고, 저장할 필드에는 final을 사용할 수 없다.</p>
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
<td align="center">JPA에서 사용할 엔티티 이름을 지정한다. 보통 기본값인 클래스 이름을 사용한다.</td>
<td align="center">설정하지 않으면 클래스 이름을 그대로 사용한다.</td>
</tr>
</tbody></table>
</li>
</ul>
<h4 id="12-table">1.2. @Table</h4>
<ul>
<li><p>@Table은 엔티티와 매핑할 테이블을 지정하며, 이를 생략하면 매핑한 엔티티 이름을 테이블 이름으로 사용한다.</p>
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
<td align="center">매핑할 테이블 이름</td>
<td align="center">엔티티 이름을 사용한다.</td>
</tr>
<tr>
<td align="center">catalog</td>
<td align="center">catalog 기능이 있는 데이터베이스에서 catalog를 매핑한다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">schema</td>
<td align="center">schema 기능이 있는 데이터베이스에서 schema를 매핑한다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">uniqueConstraints(DDL)</td>
<td align="center">DDL 생성 시에 유니크 제약조건을 만든다. 2개 이상의 복합 유니크 제약조건도 만들 수 있다. 참고로 이 기능은 스키마 자동 생성 기능을 사용해서 DDL을 만들 때만 사용된다.</td>
<td align="center"></td>
</tr>
</tbody></table>
</li>
</ul>
<h3 id="2-데이터베이스-스키마-자동-생성">2. 데이터베이스 스키마 자동 생성</h3>
<ul>
<li><p>JPA는 데이터베이스 스키마를 자동으로 생성하는 기능을 지원한다. 클래스의 매핑 정보를 통해 어떤 테이블에 어떤 컬럼을 사용할지 알 수 있으며, JPA는 이러한 매핑 정보와 데이터베이스 방언을 사용하여 데이터베이스 스키마를 생성한다.</p>
</li>
<li><p>hibernate.hbm2ddl.auto 속성</p>
<pre><code class="language-java">&lt;property name=&quot;hibernate.hbm2ddl.auto&quot; value=&quot;create&quot;/&gt;</code></pre>
<ul>
<li>persistence.xml에 다음 속성을 추가하면 데이터베이스 스키마가 자동으로 생성된다.</li>
</ul>
<table>
<thead>
<tr>
<th align="center">옵션</th>
<th align="center">설명</th>
</tr>
</thead>
<tbody><tr>
<td align="center">create</td>
<td align="center">기존 테이블을 삭제하고 새로 생성한다. DROP + CREATE</td>
</tr>
<tr>
<td align="center">create-drop</td>
<td align="center">create 속성에 추가로 애플리케이션을 종료할 때 생성한 DDL을 제거한다. DROP + CREATE + DROP update    데이터베이스 테이블과 엔티티 매핑정보를 비교해서 변경 사항만 수정한다.</td>
</tr>
<tr>
<td align="center">validate</td>
<td align="center">데이터베이스 테이블과 엔티티 매핑정보를 비교해서 차이가 있으면 경고를 남기고 애플리케이션을 실행하지 않는다. 이 설정은 DDL을 수정하지 않는다.</td>
</tr>
<tr>
<td align="center">none</td>
<td align="center">자동 생성 기능을 사용하지 않으려면 hibernate.hbm2ddl.auto 속성 자체를 삭제하거나 유효하지 않은 옵션 값을 주면 된다. (none은 유효하지 않은 옵션 값이다.)</td>
</tr>
<tr>
<td align="center"><br /></td>
<td align="center"></td>
</tr>
<tr>
<td align="center">- HBM2DDL 주의사항</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">- 운영 장비에서는 절대 create, create-drop, update 사용하면 안 된다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">- 개발 초기 단계는 create 또는 update</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">- 테스트 서버는 update 또는 validate</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">- 스테이징과 운영 서버는 validate 또는 none</td>
<td align="center"></td>
</tr>
</tbody></table>
</li>
</ul>
<h4 id="21-ddl-생성-기능">2.1. DDL 생성 기능</h4>
<pre><code class="language-java">//not null 제약조건과 문자의 크기를 지정할 수 있다.
@Column(name = &quot;name&quot;, nullable = false, length = 10)
private String username;

//유니크 제약조건을 추가할 수 있다.
@Table(name = &quot;MEMBER&quot;, uniqueConstraints = {@UniqueConstraint(
    name = &quot;NAME_AGE_UNIQUE&quot;,
    columnNames = {&quot;NAME&quot;, &quot;AGE&quot;}
)})
public class Member {

    @Id
    private Long id;

    @Column(name = &quot;name&quot;)
    private String name;

    private Integer age;

}</code></pre>
<ul>
<li>이런 기능들은 단지 DDL을 자동 생성할 때만 사용되며, JPA의 실행 로직에는 영향을 주지 않는다. 따라서 스키마 자동 생성 기능을 사용하지 않고 직접 DDL을 작성한다면 이러한 기능을 사용할 이유가 없다.</li>
</ul>
<h3 id="3-필드와-컬럼-매핑">3. 필드와 컬럼 매핑</h3>
<table>
  <thead>
    <tr>
      <th>분류</th>
      <th>매핑 어노테이션</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="5">필드와 컬럼 매핑</td>
      <td>@Column</td>
      <td>컬럼을 매핑</td>
    </tr>
    <tr>
      <td>@Enumerated</td>
      <td>자바의 enum 타입을 매핑</td>
    </tr>
    <tr>
      <td>@Temporal</td>
      <td>날짜 타입을 매핑</td>
    </tr>
    <tr>
      <td>@Lob</td>
      <td>BLOB, CLOB 타입을 매핑</td>
    </tr>
    <tr>
      <td>@Transient</td>
      <td>특정 필드를 데이터베이스에 매핑하지 않는다.</td>
    </tr>
    <tr>
      <td>기타</td>
      <td>@Access</td>
      <td>JPA가 엔티티에 접근하는 방식을 지정</td>
    </tr>
  </tbody>
</table>

<pre><code class="language-java">private LocalDate testLocalDate;
private LocalDateTime testLocalDateTime;</code></pre>
<p>이와 같이 LocalDate를 사용하는 경우 @Temporal 매핑 어노테이션이 필요 없다.</p>
<h4 id="31-column">3.1. @Column</h4>
<ul>
<li><p>@Column은 객체 필드를 테이블 컬럼에 매핑한다. 이 어노테이션에서 name과 nullable 속성이 주로 사용되며, 나머지 속성들은 잘 사용되지 않는다.</p>
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
<td align="center">필드와 매핑할 테이블의 컬럼 이름</td>
<td align="center">객체의 필드 이름</td>
</tr>
<tr>
<td align="center">insertable (거의 사용하지 않음)</td>
<td align="center">엔티티 저장시 이 필드도 같이 저장한다. false로 설정하면 이 필드는 데이터베이스에 저장하지 않는다.</td>
<td align="center">true</td>
</tr>
<tr>
<td align="center">updatable (거의 사용하지 않음)</td>
<td align="center">엔티티 수정 시 이 필드도 같이 수정 false로 설정하면 데이터베이스에 수정하지 않는다.</td>
<td align="center">true</td>
</tr>
<tr>
<td align="center">table (거의 사용하지 않음)</td>
<td align="center">하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용. 지정한 필드를 다른 테이블에 매핑할 수 있다.</td>
<td align="center">현재 클래스가 매핑된 테이블</td>
</tr>
<tr>
<td align="center">nullable(DDL)</td>
<td align="center">null 값의 허용 여부를 설정한다. false로 설정하면 DDL 생성 시에 not null 제약조건이 붙는다.</td>
<td align="center">true</td>
</tr>
<tr>
<td align="center">unique(DDL)(사용하지않는다. @Table 속성을 사용해서 unique 기능을 사용한다.)</td>
<td align="center">@Table의 uniqueContraints와 같지만 한 칼럼에 간단히 유니크 제약조건을 걸 때 사용한다. 만약 두 컬럼 이상을 사용해서 유니크 제약조건을 사용하려면 클래스 레벨에서 @Table.uniqueConstraints를 사용해야 한다.</td>
<td align="center"></td>
</tr>
<tr>
<td align="center">columnDefinition(DDL)</td>
<td align="center">데이터베이스 컬럼 정보를 직접 줄 수 있다.</td>
<td align="center">필드의 자바 타입과 방언 정보를 사용해서 적절한 컬럼 타입을 생성한다</td>
</tr>
<tr>
<td align="center">lenght(DDL)</td>
<td align="center">문자 길이 제약조건, String 타입에만 사용</td>
<td align="center">255</td>
</tr>
<tr>
<td align="center">precision, scale(DDL)</td>
<td align="center">BigDemical 타입에서 사용한다.(BigInteger도 사용 할 수 있다). precision은 소수점을 포함한 전체 자릿수를 scale은 소수의 자릿수다. 참고로 double, float 타입에는 적용되지 않는다. 아주 큰 숫자나 정밀한 소수를 다루어야 할 때만 사용한다.</td>
<td align="center">precision=19, scale=2</td>
</tr>
</tbody></table>
<pre><code class="language-java">@Column(nullable = false)
private String data;
// data varchar(255) not null

@Column(columnDefinition = &quot;varchar(100) default 'EMPTY'&quot;)
private String data;
// data varchar(100) default 'EMPTY'

@Column(length = 400)
private String data;
// data varchar(400)</code></pre>
<h4 id="32-column-생략">3.2. @Column 생략</h4>
</li>
<li><p>@Column을 생략하게 되면 어떻게 될까?</p>
<pre><code class="language-java">// 기본 타입에는 null 값을 입력할 수 없다.
int data1;
data1 integer not null //DDL

//객체 타입 일 때는 null 값이 허용된다.
Integer data2;
data2 integer //DDL

// 
@Column
int data3
data3 integer //DDL</code></pre>
<h4 id="33-enumerated">3.3. @Enumerated</h4>
</li>
<li><p>자바의 enum 타입을 매핑할 때 사용한다.</p>
<table>
<thead>
<tr>
<th align="center">속성</th>
<th align="center">기능</th>
<th align="center">기본값</th>
</tr>
</thead>
<tbody><tr>
<td align="center">value</td>
<td align="center">EnumType.ORDINAL: enum 순서를 데이터베이스에 저장EnumType.STRING: enum 이름을 데이터베이스에 저장</td>
<td align="center">EnumType.ORDINAL</td>
</tr>
</tbody></table>
<ul>
<li>EnumType.ORDINAL은 enum에 정의된 순서대로 ADMIN은 0, USER는 1 값이 데이터베이스에 저장된다. 이 방식의 장점은 데이터베이스에 저장되는 데이터 크기가 작다는 것이지만, 단점으로는 이미 저장된 enum의 순서를 변경할 수 없다는 점이 있다. 반면, EnumType.STRING은 enum 이름 그대로 ADMIN은 'ADMIN', USER는 'USER'라는 문자로 데이터베이스에 저장된다. 이 방식의 장점은 저장된 enum의 순서가 바뀌거나 enum이 추가되어도 안전하다는 것이지만, 단점으로는 데이터베이스에 저장되는 데이터 크기가 ORDINAL에 비해 크다는 점이 있다. ORDINAL 방식의 단점이 크기 때문에 항상 EnumType.STRING을 사용하는 것이 권장된다.<pre><code class="language-java">public enum OrderStatus {
  ORDER, CANCEL;
}
@Enumerated(EnumType.STRING)
private OrderStatus status;</code></pre>
<h4 id="34-temporal">3.4. @Temporal</h4>
</li>
<li>날짜 타입(java.util.Date, java.util.Calendar)를 매핑할 때 사용한다.</li>
</ul>
<table>
<thead>
<tr>
<th align="center">속성</th>
<th align="center">기능</th>
<th align="center">기본값</th>
</tr>
</thead>
<tbody><tr>
<td align="center">value</td>
<td align="center">Temporal.DATE : 날짜, 데이터베이스 date 타입과 매핑<br />TemperalType.TIME : 시간, 데이터베이스 time 타입과 매핑<br />TemperalType.TIMESTAMP : 날짜와 시간, 데이터베이스 timestamp 타입과 매핑</td>
<td align="center">TemporalType은 필수로 지정해야 한다.</td>
</tr>
</tbody></table>
<pre><code class="language-java">@Temporal(TemporalType.DATE) // (ex : 2013-10-11)
private Date date;
@Temporal(TemporalType.TIME) // (ex : 11:11:11)
private Date time;
@Temporal(TemporalType.TIMESTAMP) // (ex : 2013-10-11 11:11:11)
private Date timestamp;

//==생성된 DDL==/
date date,
time time,
timestamp timestamp</code></pre>
</li>
<li><p>자바의 Date 타입에는 년월일 시분초가 있지만, 데이터베이스에는 date, time, timestamp(날짜와 시간)라는 세 가지 타입이 별도로 존재한다. @Temporal을 생략하면 자바의 Date와 가장 유사한 DDL이 생성되며, 이는 데이터베이스에 따라 다음과 같이 다르게 매핑된다: </p>
<ul>
<li>datetime : MYSQL</li>
<li>timestamp : H2, 오라클, PostgreSQL</li>
</ul>
<pre><code class="language-java">import java.time.LocalDate;
import java.time.LocalDateTime;
private LocalDate testLocalDate;
private LocalDateTime testLocalDateTime;</code></pre>
<ul>
<li>java.time 패키지의 LocalDate와 LocalDateTime을 사용할 때는 @Temperal 어노테이션이 필요 없다.</li>
</ul>
</li>
</ul>
<h4 id="35-lob">3.5. @Lob</h4>
<ul>
<li><p>@Lob 어노테이션은 데이터베이스의 BLOB, CLOB 타입과 매핑하는 데 사용된다. @Lob에는 지정할 수 있는 속성이 없으며, 매핑하는 필드 타입이 문자일 경우 CLOB으로 매핑된다. CLOB은 String, char[], java.sql.CLOB 타입과 매핑되고, BLOB은 byte[], java.sql.BLOB 타입과 매핑된다.</p>
<pre><code class="language-java">@Lob
private String lobString;
@Lob
private byte[] lobByte;

//오라클
lobString clob;
lobByte blob;

//MySQL
lobString longtext,
lobByte longblob,</code></pre>
<h4 id="36-transient">3.6. @Transient</h4>
</li>
<li><p>@Transient 어노테이션은 &quot;이 필드는 매핑하지 않는다&quot;를 선언해 준다. 즉, 해당 필드는 데이터베이스에 저장되지 않으며, 데이터베이스로부터 조회되지도 않는다.</p>
<pre><code class="language-java">@Transient
private int temp;</code></pre>
</li>
</ul>
<ol start="4">
<li>기본 키 매핑</li>
</ol>
<ul>
<li><p>직접 할당 : 기본 키를 애플리케이션에서 직접 할당한다.</p>
<ul>
<li>@Id 어노테이션만 사용하고 개발자가 직접 할당한다.</li>
</ul>
</li>
<li><p>자동 생성 : 대리 키 사용 방식 - @GeneratedValue 어노테이션을 추가해야 한다.</p>
<ul>
<li>IDENTITY : 기본 키 생성을 데이터베이스에 위임한다.</li>
<li>SEQUENCE : 데이터베이스 시퀀스를 사용해서 기본 키를 할당한다.</li>
<li>TABLE : 키 생성 테이블을 사용한다.
(1) 기본 키 직접 할당 전략</li>
</ul>
</li>
<li><p>@Id로 매핑한다.</p>
</li>
<li><p>@Id 적용 가능 자바 타입</p>
<ul>
<li>자바 기본형</li>
<li>자바 래퍼형(Wrapper)</li>
<li>String</li>
<li>java.util.Date</li>
<li>java.sql.Date</li>
<li>java.math.BigDecimal</li>
<li>java.math.BigInteger<pre><code class="language-java">@Id
private Long id;
</code></pre>
</li>
</ul>
<p>Board board = new Board();
board.setId(1L);
em.persist(board);</p>
<pre><code></code></pre></li>
</ul>
<p>(2) IDENTITY 전략</p>
<ul>
<li>기본 키 생성을 데이터베이스에 위임하는 전략</li>
<li>MySQL, PostgreSQL, SQL Server, DB2에서 사용한다.</li>
<li>MySQL의 AUTO_INCREMENT 기능은 데이터베이스가 기본 키를 자동으로 생성해준다.</li>
<li>IDENTITY 전략은 AUTO_INCREMENT를 사용하여 데이터베이스에 값을 저장하고 나서야 기본 키 값을 구한다.<pre><code class="language-java">@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
private static void logic(EntityManager em) {
      Member member = new Member();
      em.persist(member);
      System.out.println(member.getId());
}</code></pre>
</li>
<li>엔티티가 영속 상태가 되려면 식별자가 반드시 필요하다.</li>
<li>하지만 IDENTITY 식별자 생성 전략은 엔티티를 데이터베이스에 저장해야 식별자를 구할 수 있으므로 em.persist()를 호출하는 즉시 INSERT SQL이 데이터베이스에 전달된다.</li>
<li>이 전략은 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.</li>
</ul>
<p>(3) SEQUENCE 전략</p>
<ul>
<li><p>데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트이다.</p>
</li>
<li><p>SEQUENCE 전략은 이 시퀀스를 사용해서 기본 키를 생성한다.</p>
</li>
<li><p>오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용할 수 있다.</p>
<pre><code class="language-java">@Entity
@SequenceGenerator(
      name = &quot;BOARD_SEQ_GENERATOR&quot;,
      sequenceName = &quot;BOARD_SEQ&quot;, //매핑할 데이터베이스 시퀀스 이름
      initialValue = 1, allocationSize = 1
)
public class Member {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = &quot;BOARD_SEQ_GENERATOR&quot;)
  private Long id;
</code></pre>
</li>
</ul>
<p>}</p>
<pre><code>- 데이터베이스 시퀀스를 매핑한다.
  - @SequenceGenerator를 사용해서BOARD_SEQ_GENERATOR라는 시퀀스 생성기를 등록
- sequenceName 속성의 이름으로 BOARD_SEQ를 지정
JPA는 이 시퀀스 생성기를 데이터베이스의 BOARD_SEQ 시퀀스와 매핑한다.
- SEQUENCE 전략은 em.persist()를 호출할 때 먼저 데이터베이스 시퀀스를 사용해서 식별자를 조회한다.
em.persist() 호출 시 &quot;call next value for BOARD_SEQ&quot; 쿼리가 나간다.

*SequenceGenerator 속성 정리

|속성|기능|기본값|
|:-:|:-:|:-:|
|name|식별자 생성기 이름|필수|
|sequenceName|데이터베이스에 등록되어 있는 시퀀스 이름|hibernate_sequence|
|initialValue|DDL 생성 시에만 사용됨. 시퀀스 DDL을 생성할 때 처음 시작하는 수를 지정한다.|1|
|allocationSize|시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용된다.)|50|
|catalog, schema|데이터베이스 catalog, schema 이름||

(4) TABLE 전략
- TABLE 전략은 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만들어 데이터베이스 시퀀스를 흉내 내는 전략이다.
- 이 전략은 테이블을 사용하므로 모든 데이터베이스에 적용할 수 있다.
- 실무에서 잘 사용하지 않는다.
(5) AUTO 전략
- GenrerationType.AUTO는 선택한 데이터베이스 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동으로 선택한다.
  - 오라클 : SEQUENCE
  - MySQL : IDENTITY
```java
@Entity
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

}</code></pre><ul>
<li>@GeneratedValue.strategy의 기본값은 AUTO이다. 
strategy = GenerationType.AUTO 생략 가능!</li>
<li>AUTO를 사용할 때 SEQUENCE 전략이 선택되면 시퀀스나 키 생성용 테이블을 미리 만들어 두어야 한다.</li>
<li>만약 스키마 자동 생성 기능을 사용한다면 하이버네이트가 기본값을 사용해서 적절한 시퀀스를 만들어준다.</li>
</ul>