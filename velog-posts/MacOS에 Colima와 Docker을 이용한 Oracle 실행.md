<h3 id="macos에서-homebrew를-사용해-colima와-docker를-설치하고-oracle을-실행하는-방법">macOS에서 Homebrew를 사용해 Colima와 Docker를 설치하고 Oracle을 실행하는 방법</h3>
<p>Oracle 데이터베이스는 macOS에서 직접 설치할 수 없기 때문에, Docker를 활용해 설치하는 방법이 유용하다. Docker를 사용하면 컨테이너화된 Oracle 인스턴스를 쉽게 실행하고 관리할 수 있다. 이 글에서는 Homebrew를 사용해 Colima와 Docker를 설치한 다음, Docker를 이용해 Oracle을 설치하고 포트 포워딩을 설정하여 macOS에서 실행하는 방법을 자세히 설명하겠다.</p>
<hr />
<h3 id="1-homebrew-설치">1. Homebrew 설치</h3>
<p>Homebrew는 macOS에서 패키지를 설치하고 관리할 수 있는 패키지 관리자이다. Homebrew가 설치되어 있는지 확인하고, 설치되어 있지 않다면 다음 명령어를 통해 설치한다.</p>
<pre><code class="language-sh">/bin/bash -c &quot;$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)&quot;</code></pre>
<p>설치가 완료되면 Homebrew가 제대로 설치되었는지 확인한다.</p>
<pre><code class="language-sh">brew --version</code></pre>
<hr />
<h3 id="2-colima-설치">2. Colima 설치</h3>
<p>Colima는 macOS에서 Docker를 실행하기 위한 경량화된 Linux VM이다. Colima를 사용하면 macOS에서 쉽게 Docker 컨테이너를 실행할 수 있다. Colima를 설치하려면 다음 명령어를 사용한다.</p>
<pre><code class="language-sh">brew install colima</code></pre>
<p>설치가 완료되면 Colima를 시작한다.</p>
<pre><code class="language-sh">colima start</code></pre>
<p>Colima가 성공적으로 시작되었는지 확인하려면 다음 명령어를 실행한다.</p>
<pre><code class="language-sh">colima status</code></pre>
<hr />
<h3 id="3-docker-설치">3. Docker 설치</h3>
<p>Homebrew를 사용해 Docker를 설치한다. Colima와 함께 Docker를 사용하면 macOS에서 네이티브 Docker 경험을 제공받을 수 있다.</p>
<pre><code class="language-sh">brew install docker</code></pre>
<p>설치가 완료되면 Docker가 제대로 설치되었는지 확인한다.</p>
<pre><code class="language-sh">docker --version</code></pre>
<p>이제 Docker를 사용할 준비가 되었다.</p>
<hr />
<h3 id="4-oracle-데이터베이스-docker-이미지-다운로드">4. Oracle 데이터베이스 Docker 이미지 다운로드</h3>
<p>Oracle의 공식 Docker 이미지를 사용해 Oracle 데이터베이스를 실행할 수 있다. Docker Hub에서 Oracle 데이터베이스 이미지를 다운로드한다. 이 예제에서는 <code>oracleinanutshell/oracle-xe-11g</code> 이미지를 사용한다.</p>
<pre><code class="language-sh">docker pull oracleinanutshell/oracle-xe-11g</code></pre>
<p>다운로드가 완료되면 다음 명령어를 통해 이미지가 제대로 다운로드되었는지 확인할 수 있다.</p>
<pre><code class="language-sh">docker images</code></pre>
<hr />
<h3 id="5-oracle-데이터베이스-컨테이너-실행">5. Oracle 데이터베이스 컨테이너 실행</h3>
<p>Oracle 데이터베이스 컨테이너를 실행할 때 포트 포워딩을 설정하여 macOS에서 접근할 수 있도록 한다. 일반적으로 Oracle 데이터베이스는 1521 포트를 사용한다.</p>
<pre><code class="language-sh">docker run -d --name oracle-xe \
  -p 1521:1521 -p 8080:8080 \
  oracleinanutshell/oracle-xe-11g</code></pre>
<p>이 명령어는 다음을 수행한다:</p>
<ul>
<li><code>-d</code> 옵션은 컨테이너를 백그라운드에서 실행한다.</li>
<li><code>--name oracle-xe</code> 옵션은 컨테이너 이름을 <code>oracle-xe</code>로 설정한다.</li>
<li><code>-p 1521:1521</code> 옵션은 호스트의 1521 포트를 컨테이너의 1521 포트로 포워딩한다.</li>
<li><code>-p 8080:8080</code> 옵션은 호스트의 8080 포트를 컨테이너의 8080 포트로 포워딩한다.</li>
<li><code>oracleinanutshell/oracle-xe-11g</code>는 사용할 Oracle Docker 이미지이다.</li>
</ul>
<p>컨테이너가 실행 중인지 확인하려면 다음 명령어를 사용한다.</p>
<pre><code class="language-sh">docker ps</code></pre>
<p>이 명령어는 현재 실행 중인 컨테이너 목록을 표시한다. <code>oracle-xe</code> 컨테이너가 실행 중이어야 한다.</p>
<hr />
<h3 id="6-oracle-데이터베이스에-접속하기">6. Oracle 데이터베이스에 접속하기</h3>
<p>Oracle 데이터베이스 컨테이너가 실행되고 있으면, 이제 데이터베이스에 접속할 수 있다. SQL*Plus를 사용해 데이터베이스에 접속하거나, Oracle SQL Developer와 같은 GUI 도구를 사용할 수 있다.</p>
<h4 id="sqlplus를-사용한-접속">SQL*Plus를 사용한 접속</h4>
<p>먼저, Docker 컨테이너 내부로 접속한다.</p>
<pre><code class="language-sh">docker exec -it oracle-xe bash</code></pre>
<p>그런 다음, SQL*Plus를 실행하여 Oracle 데이터베이스에 접속한다.</p>
<pre><code class="language-sh">sqlplus sys as sysdba</code></pre>
<p>기본적으로 Oracle XE 이미지는 다음과 같은 사용자 이름과 암호를 사용한다.</p>
<ul>
<li>사용자 이름: <code>sys</code></li>
<li>암호: <code>oracle</code></li>
</ul>
<h4 id="oracle-sql-developer를-사용한-접속">Oracle SQL Developer를 사용한 접속</h4>
<ol>
<li>Oracle SQL Developer를 실행한다.</li>
<li>새로운 연결을 만든다.</li>
<li>다음과 같이 연결 정보를 입력한다.<ul>
<li>호스트 이름: <code>localhost</code></li>
<li>포트: <code>1521</code></li>
<li>서비스 이름: <code>XE</code></li>
<li>사용자 이름: <code>sys</code></li>
<li>암호: <code>oracle</code></li>
<li>역할: <code>SYSDBA</code></li>
</ul>
</li>
</ol>
<p>연결을 테스트하고 성공하면 연결을 저장하고 접속한다.</p>
<hr />
<h3 id="7-oracle-데이터베이스-컨테이너-관리">7. Oracle 데이터베이스 컨테이너 관리</h3>
<p>Oracle 데이터베이스 컨테이너를 중지하거나 다시 시작하려면 다음 명령어를 사용한다.</p>
<h4 id="컨테이너-중지">컨테이너 중지</h4>
<pre><code class="language-sh">docker stop oracle-xe</code></pre>
<h4 id="컨테이너-다시-시작">컨테이너 다시 시작</h4>
<pre><code class="language-sh">docker start oracle-xe</code></pre>
<h4 id="컨테이너-삭제">컨테이너 삭제</h4>
<p>더 이상 필요하지 않은 경우 컨테이너를 삭제할 수 있다.</p>
<pre><code class="language-sh">docker rm oracle-xe</code></pre>
<hr />
<h3 id="8-colima-및-docker-삭제">8. Colima 및 Docker 삭제</h3>
<h4 id="colima-삭제">Colima 삭제</h4>
<p>Colima를 삭제하려면 다음 명령어를 사용한다.</p>
<pre><code class="language-sh">brew uninstall colima</code></pre>
<h4 id="docker-삭제">Docker 삭제</h4>
<p>Docker를 삭제하려면 다음 명령어를 사용한다.</p>
<pre><code class="language-sh">brew uninstall docker</code></pre>
<h4 id="homebrew-삭제">Homebrew 삭제</h4>
<p>Homebrew를 완전히 삭제하려면 다음 명령어를 사용한다.</p>
<pre><code class="language-sh">/bin/bash -c &quot;$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)&quot;</code></pre>
<hr />
<p>이 가이드를 따르면 macOS에서 Homebrew를 사용해 Colima와 Docker를 설치하고, Docker를 통해 Oracle 데이터베이스를 실행할 수 있다. 이 방법은 macOS에서 직접 Oracle을 설치할 수 없기 때문에 유용하며, Docker 컨테이너를 사용하면 쉽게 데이터베이스 인스턴스를 관리할 수 있다.</p>