<p>DAO, DTO, VO는 자바 애플리케이션 개발에서 자주 사용되는 디자인 패턴과 개념이다. 이들 각각은 데이터의 접근, 전달, 표현을 체계적으로 관리하기 위해 사용된다. 아래에서는 각 개념에 대해 자세히 설명하고, 예제 코드를 통해 이해를 돕고자 한다.</p>
<h3 id="1-dao-data-access-object">1. DAO (Data Access Object)</h3>
<p><strong>DAO</strong>는 데이터베이스에 접근하고 조작하는 역할을 담당하는 객체이다. 데이터베이스 연산(삽입, 조회, 업데이트, 삭제)을 캡슐화하여 데이터베이스와 애플리케이션의 비즈니스 로직을 분리한다. 이를 통해 코드의 유지보수성과 재사용성을 높일 수 있다.</p>
<h4 id="주요-특징">주요 특징</h4>
<ul>
<li>데이터베이스와의 통신을 캡슐화한다.</li>
<li>CRUD(Create, Read, Update, Delete) 작업을 수행한다.</li>
<li>비즈니스 로직과 데이터 접근 로직을 분리한다.</li>
</ul>
<h4 id="예제-코드">예제 코드</h4>
<pre><code class="language-java">import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

// DTO 클래스 정의
class UserDTO {
    private int id;
    private String name;
    private String email;

    // Getters and Setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}

// DAO 클래스 정의
class UserDAO {
    private Connection getConnection() throws SQLException {
        String url = &quot;jdbc:mysql://localhost:3306/mydatabase&quot;;
        String username = &quot;root&quot;;
        String password = &quot;password&quot;;
        return DriverManager.getConnection(url, username, password);
    }

    public UserDTO getUserById(int id) {
        UserDTO user = null;
        String query = &quot;SELECT * FROM users WHERE id = ?&quot;;

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(query)) {
            pstmt.setInt(1, id);
            ResultSet rs = pstmt.executeQuery();

            if (rs.next()) {
                user = new UserDTO();
                user.setId(rs.getInt(&quot;id&quot;));
                user.setName(rs.getString(&quot;name&quot;));
                user.setEmail(rs.getString(&quot;email&quot;));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }

        return user;
    }

    public void addUser(UserDTO user) {
        String query = &quot;INSERT INTO users (name, email) VALUES (?, ?)&quot;;

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(query)) {
            pstmt.setString(1, user.getName());
            pstmt.setString(2, user.getEmail());
            pstmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

public class DAODemo {
    public static void main(String[] args) {
        UserDAO userDAO = new UserDAO();

        // 새로운 사용자 추가
        UserDTO newUser = new UserDTO();
        newUser.setName(&quot;John Doe&quot;);
        newUser.setEmail(&quot;john.doe@example.com&quot;);
        userDAO.addUser(newUser);

        // 사용자 정보 조회
        UserDTO user = userDAO.getUserById(1);
        if (user != null) {
            System.out.println(&quot;User ID: &quot; + user.getId());
            System.out.println(&quot;User Name: &quot; + user.getName());
            System.out.println(&quot;User Email: &quot; + user.getEmail());
        } else {
            System.out.println(&quot;User not found.&quot;);
        }
    }
}</code></pre>
<h3 id="2-dto-data-transfer-object">2. DTO (Data Transfer Object)</h3>
<p><strong>DTO</strong>는 계층 간 데이터 전달을 위한 객체이다. 주로 데이터를 캡슐화하고, 서비스나 컨트롤러 계층에서 데이터를 교환하는 데 사용된다. DTO는 데이터베이스의 엔티티와는 독립적으로 사용되며, 네트워크 통신이나 API 호출 시 데이터의 포맷을 맞추는 역할을 한다.</p>
<h4 id="주요-특징-1">주요 특징</h4>
<ul>
<li>데이터 전송을 위한 객체이다.</li>
<li>일반적으로 getter와 setter 메서드를 포함한다.</li>
<li>비즈니스 로직이 포함되지 않는다.</li>
</ul>
<h4 id="예제-코드-1">예제 코드</h4>
<p>위 <code>UserDTO</code> 클래스가 DTO의 좋은 예이다. 사용자 정보를 저장하고, 데이터 전송 시 사용된다.</p>
<h3 id="3-vo-value-object">3. VO (Value Object)</h3>
<p><strong>VO</strong>는 값 자체를 표현하는 객체이다. VO는 변경 불가능(Immutable)하며, 주로 값의 동등성(equality)을 비교하는 데 사용된다. 동일성을 비교하기 위해 <code>equals()</code>와 <code>hashCode()</code> 메서드를 재정의한다.</p>
<h4 id="주요-특징-2">주요 특징</h4>
<ul>
<li>변경 불가능한 객체이다.</li>
<li>값을 표현하며, 동일성 비교를 위해 <code>equals()</code>와 <code>hashCode()</code>를 재정의한다.</li>
<li>주로 특정 비즈니스 값을 나타낸다.</li>
</ul>
<h4 id="예제-코드-2">예제 코드</h4>
<pre><code class="language-java">public class UserVO {
    private final int id;
    private final String name;
    private final String email;

    public UserVO(int id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        UserVO userVO = (UserVO) o;
        return id == userVO.id &amp;&amp;
                name.equals(userVO.name) &amp;&amp;
                email.equals(userVO.email);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, email);
    }

    @Override
    public String toString() {
        return &quot;UserVO{&quot; +
                &quot;id=&quot; + id +
                &quot;, name='&quot; + name + '\'' +
                &quot;, email='&quot; + email + '\'' +
                '}';
    }
}

public class VODemo {
    public static void main(String[] args) {
        UserVO user1 = new UserVO(1, &quot;Alice&quot;, &quot;alice@example.com&quot;);
        UserVO user2 = new UserVO(1, &quot;Alice&quot;, &quot;alice@example.com&quot;);

        System.out.println(user1);
        System.out.println(&quot;User1 equals User2: &quot; + user1.equals(user2));
    }
}</code></pre>
<h3 id="결론">결론</h3>
<p>DAO, DTO, VO는 자바 애플리케이션에서 데이터 관리를 체계적으로 수행하기 위한 중요한 패턴이다. DAO는 데이터베이스와의 상호작용을 캡슐화하고, DTO는 계층 간 데이터 전달을 담당하며, VO는 변경 불가능한 값 객체로서 값의 동일성을 비교한다. </p>