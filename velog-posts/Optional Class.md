<h3 id="자바의-optional에-대하여">자바의 Optional에 대하여</h3>
<p>자바 8에서 도입된 <code>Optional</code> 클래스는 null 참조를 다루기 위한 대안으로 제공된다. <code>Optional</code>은 값의 존재 또는 부재를 나타내며, null을 명시적으로 처리하는데 도움을 준다. 이는 코드의 가독성을 높이고, NullPointerException(NPE)을 예방하는 데 유용하다.</p>
<h4 id="optional의-주요-기능">Optional의 주요 기능</h4>
<ol>
<li><strong>값의 존재 확인</strong>: 값을 안전하게 감싸서, 값이 존재하는지 확인할 수 있다.</li>
<li><strong>기본값 제공</strong>: 값이 없을 경우 기본값을 제공할 수 있다.</li>
<li><strong>값의 변환</strong>: 존재하는 값에 대해 함수를 적용할 수 있다.</li>
<li><strong>값의 필터링</strong>: 조건을 만족하는 값만 필터링할 수 있다.</li>
</ol>
<h3 id="optional-클래스의-사용법">Optional 클래스의 사용법</h3>
<h4 id="1-optional-객체-생성">1. Optional 객체 생성</h4>
<pre><code class="language-java">Optional&lt;String&gt; optionalEmpty = Optional.empty();
Optional&lt;String&gt; optionalOf = Optional.of(&quot;Hello, World!&quot;);
Optional&lt;String&gt; optionalOfNullable = Optional.ofNullable(null);</code></pre>
<ul>
<li><code>Optional.empty()</code>: 비어있는 Optional 객체를 생성한다.</li>
<li><code>Optional.of(value)</code>: null이 아닌 값을 감싸는 Optional 객체를 생성한다. null을 전달하면 <code>NullPointerException</code>이 발생한다.</li>
<li><code>Optional.ofNullable(value)</code>: null 값을 허용하며, 값이 null이면 비어있는 Optional 객체를 생성한다.</li>
</ul>
<h4 id="2-값의-존재-여부-확인">2. 값의 존재 여부 확인</h4>
<pre><code class="language-java">if (optionalOf.isPresent()) {
    System.out.println(&quot;Value is present: &quot; + optionalOf.get());
} else {
    System.out.println(&quot;Value is not present&quot;);
}</code></pre>
<ul>
<li><code>isPresent()</code>: 값이 존재하면 true, 그렇지 않으면 false를 반환한다.</li>
<li><code>get()</code>: 값을 반환하지만, 값이 없으면 <code>NoSuchElementException</code>을 발생시킨다. 따라서 직접 사용하기보다는 다른 안전한 메서드를 사용하는 것이 좋다.</li>
</ul>
<h4 id="3-기본값-제공">3. 기본값 제공</h4>
<pre><code class="language-java">String value = optionalEmpty.orElse(&quot;Default Value&quot;);
System.out.println(value);

String valueSupplier = optionalEmpty.orElseGet(() -&gt; &quot;Generated Default Value&quot;);
System.out.println(valueSupplier);</code></pre>
<ul>
<li><code>orElse(defaultValue)</code>: 값이 없을 경우 기본값을 반환한다.</li>
<li><code>orElseGet(supplier)</code>: 값이 없을 경우 람다 표현식이나 메서드 참조를 통해 기본값을 생성한다.</li>
</ul>
<h4 id="4-값의-변환">4. 값의 변환</h4>
<pre><code class="language-java">Optional&lt;String&gt; upperCaseValue = optionalOf.map(String::toUpperCase);
upperCaseValue.ifPresent(System.out::println);</code></pre>
<ul>
<li><code>map(function)</code>: 값이 존재하면 주어진 함수(람다)를 적용하여 변환된 값을 가진 Optional을 반환한다.</li>
</ul>
<h4 id="5-값의-필터링">5. 값의 필터링</h4>
<pre><code class="language-java">Optional&lt;String&gt; filteredValue = optionalOf.filter(value -&gt; value.contains(&quot;Hello&quot;));
filteredValue.ifPresent(System.out::println);</code></pre>
<ul>
<li><code>filter(predicate)</code>: 값이 존재하고 주어진 조건을 만족하면 그 값을 가진 Optional을 반환하고, 그렇지 않으면 비어있는 Optional을 반환한다.</li>
</ul>
<p>모든 클래스를 <code>Optional</code> 클래스로 만들 필요는 없으며, 그렇게 하는 것은 권장되지 않는다. <code>Optional</code>은 특정 상황에서 유용하게 사용할 수 있는 도구이지만, 모든 경우에 사용하기에는 적절하지 않다. 다음은 <code>Optional</code>을 언제, 어떻게 사용하는 것이 좋은지에 대한 가이드이다.</p>
<h3 id="optional-사용의-권장-사례">Optional 사용의 권장 사례</h3>
<h4 id="1-메서드-반환-타입으로-사용">1. 메서드 반환 타입으로 사용</h4>
<p><code>Optional</code>은 메서드의 반환 타입으로 사용될 때 특히 유용하다. 메서드가 값을 반환할 수도 있고, 반환하지 않을 수도 있는 경우, <code>Optional</code>을 사용하여 명시적으로 값의 부재를 나타낼 수 있다.</p>
<pre><code class="language-java">public Optional&lt;String&gt; findUserEmailById(String userId) {
    // 데이터베이스나 다른 소스로부터 사용자 이메일을 찾는다.
    String email = ...;
    return Optional.ofNullable(email);
}</code></pre>
<h4 id="2-외부-api와의-상호작용">2. 외부 API와의 상호작용</h4>
<p>외부 API나 라이브러리와 상호작용할 때, 그들이 null을 반환할 가능성이 있는 경우 <code>Optional</code>을 사용하여 null 처리를 명시적으로 할 수 있다.</p>
<pre><code class="language-java">public Optional&lt;String&gt; getExternalApiValue() {
    String value = externalApi.call();
    return Optional.ofNullable(value);
}</code></pre>
<h3 id="optional-사용의-비권장-사례">Optional 사용의 비권장 사례</h3>
<h4 id="1-필드-변수로-사용">1. 필드 변수로 사용</h4>
<p><code>Optional</code>을 클래스의 필드로 사용하는 것은 일반적으로 권장되지 않는다. 필드로 사용하면 객체의 메모리 사용량이 증가하고, 객체의 직렬화/역직렬화가 복잡해질 수 있다. 대신, 필드 자체는 null을 허용하되, 메서드 수준에서 <code>Optional</code>을 반환하는 방식이 더 적절하다.</p>
<pre><code class="language-java">class User {
    private String name;
    private String email; // Optional이 아닌 그냥 String

    public Optional&lt;String&gt; getEmail() {
        return Optional.ofNullable(email);
    }
}</code></pre>
<h4 id="2-컬렉션과-함께-사용">2. 컬렉션과 함께 사용</h4>
<p>컬렉션(<code>List</code>, <code>Set</code>, <code>Map</code> 등)은 이미 값의 부재를 처리할 수 있는 방법을 제공한다. 따라서 컬렉션 요소로 <code>Optional</code>을 사용하는 것은 중복된 작업이 될 수 있다.</p>
<pre><code class="language-java">List&lt;Optional&lt;String&gt;&gt; listOfOptionals = new ArrayList&lt;&gt;(); // 비권장
List&lt;String&gt; listOfStrings = new ArrayList&lt;&gt;(); // 권장</code></pre>
<h3 id="optional을-적절히-사용하는-방법">Optional을 적절히 사용하는 방법</h3>
<h4 id="1-기본-값-제공">1. 기본 값 제공</h4>
<p><code>Optional</code>의 값이 없는 경우 기본 값을 제공할 수 있다.</p>
<pre><code class="language-java">String email = user.getEmail().orElse(&quot;default@example.com&quot;);</code></pre>
<h4 id="2-예외-발생">2. 예외 발생</h4>
<p>값이 없을 때 예외를 발생시킬 수 있다.</p>
<pre><code class="language-java">String email = user.getEmail().orElseThrow(() -&gt; new IllegalArgumentException(&quot;Email not present&quot;));</code></pre>
<h4 id="3-값-변환">3. 값 변환</h4>
<p><code>Optional</code>을 사용하여 값이 존재할 때 변환을 수행할 수 있다.</p>
<pre><code class="language-java">Optional&lt;String&gt; email = user.getEmail().map(String::toUpperCase);
email.ifPresent(System.out::println);</code></pre>
<h3 id="optional-사용-예제">Optional 사용 예제</h3>
<p>아래 예제는 사용자의 이메일을 안전하게 처리하는 방법을 보여준다.</p>
<pre><code class="language-java">import java.util.Optional;

class User {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public Optional&lt;String&gt; getEmail() {
        return Optional.ofNullable(email);
    }
}

public class OptionalExample {
    public static void main(String[] args) {
        User userWithEmail = new User(&quot;Alice&quot;, &quot;alice@example.com&quot;);
        User userWithoutEmail = new User(&quot;Bob&quot;, null);

        // 이메일이 있는 경우 출력
        userWithEmail.getEmail().ifPresent(email -&gt; System.out.println(&quot;User email: &quot; + email));

        // 이메일이 없는 경우 기본 값 제공
        String email = userWithoutEmail.getEmail().orElse(&quot;email not provided&quot;);
        System.out.println(&quot;User email: &quot; + email);

        // 이메일이 없는 경우 예외 발생
        try {
            String emailOrThrow = userWithoutEmail.getEmail().orElseThrow(() -&gt; new IllegalArgumentException(&quot;Email not present&quot;));
            System.out.println(&quot;User email: &quot; + emailOrThrow);
        } catch (IllegalArgumentException e) {
            System.err.println(e.getMessage());
        }

        // 이메일을 대문자로 변환하여 출력
        userWithEmail.getEmail()
                .map(String::toUpperCase)
                .ifPresent(emailUpperCase -&gt; System.out.println(&quot;Uppercase email: &quot; + emailUpperCase));
    }
}</code></pre>
<h3 id="결론">결론</h3>
<p><code>Optional</code> 클래스는 null을 보다 안전하고 명시적으로 다룰 수 있는 유용한 도구이다. 이를 통해 null 참조로 인한 오류를 줄이고, 코드의 가독성을 높일 수 있다. 주로 메서드의 반환 타입으로 사용하여 값의 부재를 명시적으로 처리할 때 유용하다. 이때 필드 변수나 컬렉션 요소로 사용하는 것은 피하는 것이 좋다.</p>