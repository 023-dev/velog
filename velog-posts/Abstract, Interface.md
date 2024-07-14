<h2 id="abstract-interface">Abstract, Interface</h2>
<p>자바에서 추상 클래스와 인터페이스는 객체 지향 프로그래밍에서 다형성을 구현하는 데 중요한 역할을 한다. 이 두 가지는 모두 클래스와 메서드의 선언을 강제하여 일정한 설계 규칙을 따르게 하지만, 그 사용 목적과 기능에는 여러 차이점이 있다. 아래에서 추상 클래스와 인터페이스의 차이점, 그리고 예제 코드를 통해 각각의 사용 방법을 자세히 살펴보자.</p>
<h3 id="1-추상-클래스-abstract-class">1. 추상 클래스 (Abstract Class)</h3>
<p>추상 클래스는 인스턴스화할 수 없는 클래스이다. 추상 클래스는 하나 이상의 추상 메서드를 포함할 수 있으며, 이를 상속받는 서브클래스는 반드시 추상 메서드를 구현해야 한다. 추상 클래스는 주로 공통된 기능을 공유하는 클래스들 간의 상속 계층을 정의하는 데 사용된다.</p>
<h4 id="특징">특징</h4>
<ul>
<li>추상 클래스는 <code>abstract</code> 키워드를 사용하여 선언한다.</li>
<li>추상 메서드는 메서드 바디가 없고 서브클래스에서 구현해야 한다.</li>
<li>일반 메서드와 변수를 포함할 수 있다.</li>
<li>생성자를 가질 수 있다.</li>
<li>하나 이상의 서브클래스에서만 상속할 수 있다.</li>
</ul>
<h4 id="예제-코드">예제 코드</h4>
<pre><code class="language-java">abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void makeSound(); // 추상 메서드

    void sleep() {
        System.out.println(name + &quot; is sleeping.&quot;);
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println(name + &quot; says: Woof Woof!&quot;);
    }
}

public class AbstractClassDemo {
    public static void main(String[] args) {
        Dog dog = new Dog(&quot;Buddy&quot;);
        dog.makeSound(); // 추상 메서드 구현
        dog.sleep(); // 일반 메서드 사용
    }
}</code></pre>
<h3 id="2-인터페이스-interface">2. 인터페이스 (Interface)</h3>
<p>인터페이스는 모든 메서드가 추상 메서드로 구성된 특별한 형태의 클래스이다. 자바 8부터는 디폴트 메서드와 정적 메서드를 인터페이스에 추가할 수 있다. 인터페이스는 다중 상속을 허용하며, 클래스가 특정 행동을 구현하도록 강제하는 데 사용된다.</p>
<h4 id="특징-1">특징</h4>
<ul>
<li>인터페이스는 <code>interface</code> 키워드를 사용하여 선언한다.</li>
<li>모든 메서드는 기본적으로 추상 메서드이다. (자바 8 이후 디폴트 메서드와 정적 메서드 추가 가능)</li>
<li>변수를 선언할 수 있지만, 모든 변수는 <code>public</code>, <code>static</code>, <code>final</code>이다.</li>
<li>다중 상속을 지원한다.</li>
<li>구현 클래스는 <code>implements</code> 키워드를 사용하여 인터페이스를 구현한다.</li>
</ul>
<h4 id="예제-코드-1">예제 코드</h4>
<pre><code class="language-java">interface Animal {
    void makeSound(); // 추상 메서드

    default void sleep() { // 디폴트 메서드
        System.out.println(&quot;The animal is sleeping.&quot;);
    }
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println(&quot;Cat says: Meow Meow!&quot;);
    }
}

public class InterfaceDemo {
    public static void main(String[] args) {
        Cat cat = new Cat();
        cat.makeSound(); // 추상 메서드 구현
        cat.sleep(); // 디폴트 메서드 사용
    }
}</code></pre>
<h3 id="3-추상-클래스와-인터페이스의-차이점">3. 추상 클래스와 인터페이스의 차이점</h3>
<h4 id="공통점">공통점</h4>
<ul>
<li>둘 다 인스턴스화할 수 없으며, 상속을 통해 사용된다.</li>
<li>둘 다 클래스나 객체가 특정 메서드를 구현하도록 강제한다.</li>
</ul>
<h4 id="차이점">차이점</h4>
<table>
<thead>
<tr>
<th>추상 클래스</th>
<th>인터페이스</th>
</tr>
</thead>
<tbody><tr>
<td><code>abstract</code> 키워드로 선언된다.</td>
<td><code>interface</code> 키워드로 선언된다.</td>
</tr>
<tr>
<td>하나의 클래스만 상속할 수 있다.</td>
<td>다중 상속을 지원한다.</td>
</tr>
<tr>
<td>일반 메서드와 추상 메서드를 모두 가질 수 있다.</td>
<td>기본적으로 모든 메서드가 추상 메서드이다. (자바 8 이후 디폴트 메서드와 정적 메서드 가능)</td>
</tr>
<tr>
<td>변수와 생성자를 가질 수 있다.</td>
<td>변수는 <code>public</code>, <code>static</code>, <code>final</code>이며, 생성자를 가질 수 없다.</td>
</tr>
<tr>
<td>공통된 기능을 공유하는 클래스들의 기본 기능을 정의한다.</td>
<td>특정 행동을 구현하도록 강제한다.</td>
</tr>
</tbody></table>
<h3 id="결론">결론</h3>
<p>자바의 추상 클래스와 인터페이스는 다형성을 구현하고 코드의 재사용성을 높이는 데 중요한 도구이다. 추상 클래스는 공통된 기능을 공유하는 클래스 계층을 정의하는 데 적합하며, 인터페이스는 클래스가 특정 행동을 구현하도록 강제하는 데 유용하다.</p>