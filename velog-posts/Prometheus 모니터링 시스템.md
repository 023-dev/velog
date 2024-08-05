<h3 id="서론">서론</h3>
<p>최근에 우리는 CGV 영화 데이터를 정기적으로 크롤링하고, 이 과정을 Quartz 스케줄러로 자동화하며, 결과를 Logback을 사용하여 로깅하는 시스템을 개발하였다. 이제, 로깅된 데이터를 Prometheus로 통합하여 실시간 모니터링 및 경고 시스템을 구축하고자 한다. 이 글에서는 Prometheus와의 통합 과정과 그 중요성을 설명한다.</p>
<h3 id="prometheus-통합의-중요성">Prometheus 통합의 중요성</h3>
<p>Prometheus는 오픈 소스 모니터링 솔루션으로, 시계열 데이터 관리에 최적화되어 있으며, 복잡한 쿼리, 실시간 경고, 대시보드를 지원한다. 크롤링 시스템의 로깅 데이터를 Prometheus와 연동하면, 시스템의 성능 지표를 실시간으로 관찰하고 문제를 즉각적으로 감지할 수 있다.</p>
<h3 id="구현-과정">구현 과정</h3>
<h4 id="1-logback과-prometheus-연동">1. <strong>Logback과 Prometheus 연동</strong></h4>
<ul>
<li><strong>작업 설명</strong>: Logback으로 생성된 로깅 데이터를 Prometheus가 모니터링할 수 있도록 적절한 형식으로 전송하겠다. 이를 위해 Logback의 로거를 통해 생성된 메트릭을 Prometheus 포맷으로 변환하는 exporter를 구성하겠다.</li>
<li><strong>구현 방법</strong>:<pre><code class="language-java">import io.prometheus.client.Counter;
import io.prometheus.client.Gauge;
import io.prometheus.client.exporter.HTTPServer;
import io.prometheus.client.hotspot.DefaultExports;
</code></pre>
</li>
</ul>
<p>import java.io.IOException;</p>
<p>/**</p>
<ul>
<li><p>PrometheusConfig는 메트릭 수집을 위해 Prometheus HTTP 서버를 초기화하고 가동</p>
</li>
<li><p>/
public class PrometheusConfig {
  private static final Counter requests = Counter.build()</p>
<pre><code>      .name(&quot;requests_total&quot;).help(&quot;Total requests.&quot;).register();</code></pre><p>  // 크롤링한 페이지 수를 기록하는 카운터.
  private static final Counter crawledPages = Counter.build()</p>
<pre><code>      .name(&quot;crawled_pages_total&quot;).help(&quot;Total number of pages crawled.&quot;).register();</code></pre><p>  // 크롤링 작업의 성공/실패 횟수를 기록하는 카운터.
  private static final Counter crawlSuccesses = Counter.build()</p>
<pre><code>      .name(&quot;successful_crawls_total&quot;).help(&quot;Total number of successful crawl operations.&quot;).register();</code></pre><p>  private static final Counter crawlFailures = Counter.build()</p>
<pre><code>      .name(&quot;failed_crawls_total&quot;).help(&quot;Total number of failed crawl operations.&quot;).register();</code></pre><p>  // 크롤러 상태에 대한 게이지 측정항목 정의.(1 = 실행 중, 0 = 중지됨)
  private static final Gauge crawlerStatus = Gauge.build()</p>
<pre><code>      .name(&quot;crawler_status&quot;).help(&quot;Crawler status (1 = running, 0 = stopped)&quot;).register();</code></pre><p>  // 포트 9090에서 Prometheus HTTP 서버를 시작.
  public static void start() throws IOException {</p>
<pre><code>  DefaultExports.initialize();
  //HTTPServer server = new HTTPServer(9090);</code></pre><p>  }</p>
<p>  // 총 요청 카운터를 증가.
  public static void incrementRequests() {</p>
<pre><code>  requests.inc();</code></pre><p>  }</p>
<p>  // 크롤링한 페이지 수를 증가.
  public static void incrementCrawledPages() {</p>
<pre><code>  crawledPages.inc();</code></pre><p>  }</p>
<p>  // 성공적인 크롤링 작업을 기록.
  public static void incrementCrawlSuccess() {</p>
<pre><code>  crawlSuccesses.inc();</code></pre><p>  }</p>
<p>  // 실패한 크롤링 작업을 기록.
  public static void incrementCrawlFailure() {</p>
<pre><code>  crawlFailures.inc();</code></pre><p>  }</p>
<p>  // 크롤러 상태를 설정.
  public static void setCrawlerStatus(boolean isRunning) {</p>
<pre><code>  if (isRunning) {
      crawlerStatus.set(1);
  } else {
      crawlerStatus.set(0);
  }</code></pre><p>  }
}</p>
<pre><code></code></pre></li>
</ul>
<h4 id="2-prometheus-서버-설정">2. <strong>Prometheus 서버 설정</strong></h4>
<ul>
<li><strong>작업 설명</strong>: Prometheus 서버를 설정하여 로깅 데이터를 주기적으로 수집하겠다. Prometheus 서버는 로깅 데이터 외에도, 시스템의 다양한 성능 지표를 수집하고 저장한다.</li>
<li><strong>구현 방법</strong>:<pre><code class="language-yaml"># Prometheus configuration
global:
scrape_interval: 15s  # 메트릭을 수집하는 주기
evaluation_interval: 15s  # 룰 평가 주기
</code></pre>
</li>
</ul>
<p>alerting:  #alerting.alertmanagers: Alertmanager의 주소를 설정.
  alertmanagers:
    - static_configs:
        - targets: ['localhost:9093']  #targets: Alertmanager가 실행 중인 호스트와 포트.</p>
<p>rule_files: #rule_files: 경고 규칙 파일.</p>
<ul>
<li><p>&quot;alert_rules.yml&quot;
scrape_configs:  #scrape_configs: Prometheus가 메트릭을 수집할 엔드포인트 설정.</p>
</li>
<li><p>job_name: 'CrawlerJob'
metrics_path: '/metrics'
static_configs:</p>
<ul>
<li>targets: ['localhost:9090']  # Prometheus HTTP 서버가 실행 중인 호스트 및 포트</li>
</ul>
</li>
<li><p>job_name: 'pushgateway'
honor_labels: true
static_configs:</p>
<ul>
<li>targets: [ 'localhost:9091' ]<pre><code></code></pre></li>
</ul>
</li>
</ul>
<h4 id="3-대시보드-구성-및-경고-설정">3. <strong>대시보드 구성 및 경고 설정</strong></h4>
<ul>
<li><p><strong>작업 설명</strong>: Prometheus와 Grafana를 연동하여 크롤링 및 로깅 데이터에 대한 시각적 대시보드를 구성하겠다. 또한, 성능 이슈 또는 예외 발생 시 알림을 받을 수 있는 경고 규칙을 설정하겠다.</p>
</li>
<li><p><strong>구현 방법</strong>:</p>
<pre><code class="language-yaml">groups:  #groups: 경고 규칙 그룹.
- name: prometheus  #name: 그룹 이름.
rules:  #rules: 경고 규칙 목록.
- alert: HighRequestLatency  #alert: 경고 이름.
 expr: job:request_latency_seconds:mean5m{job=&quot;CrawlerJob&quot;} &gt; 0.5  #expr: 경고 조건을 정의하는 PromQL 표현식.
 for: 10m  #for: 조건이 충족되어야 하는 지속 시간.
 labels:  #labels: 경고에 추가할 레이블.
   severity: 'page'
 annotations:  #annotations: 경고에 대한 추가 정보.
   summary: &quot;High request latency&quot;
   description: &quot;Request latency is above 0.5s for more than 10 minutes.&quot;

- alert: CrawlerStart
 expr: up{job=&quot;crawler&quot;} == 1
 for: 1m
 labels:
   severity: info
 annotations:
   summary: &quot;Crawler started&quot;
   description: &quot;The crawler has started.&quot;

- alert: CrawlerSuccess
 expr: increase(successful_crawls_total[1m]) &gt; 0
 for: 1m
 labels:
   severity: info
 annotations:
   summary: &quot;Crawler succeeded&quot;
   description: &quot;The crawler has successfully fetched movie data.&quot;

- alert: CrawlerDown
 expr: absent(crawler_status{job=&quot;CrawlerJob&quot;} == 1)
 for: 1m
 labels:
   severity: critical
 annotations:
   summary: &quot;Crawler is down&quot;
   description: &quot;The crawler has been down for more than 1 minute.&quot;

- alert: HighErrorRate
 expr: increase(failed_crawls_total[1m]) &gt; 5  # 1분간 실패한 크롤링 횟수가 5회를 초과할 때.
 for: 1m
 labels:
   severity: critical
 annotations:
   summary: &quot;High error rate in crawling&quot;
   description: &quot;Failed crawl rate is {{ $value }} failures per minute.&quot;

- alert: NoMoviesFetched
 expr: increase(requests_total[5m]) &gt; 0 and increase(successful_crawls_total[5m]) == 0  # 5분간 요청이 있었지만 성공적인 크롤링이 없을 때.
 for: 5m
 labels:
   severity: warning
 annotations:
   summary: &quot;No movies fetched&quot;
   description: &quot;Requests were made but no movies were fetched in the last 5 minutes.&quot;</code></pre>
</li>
</ul>
<p>이렇게 설정하고 서버를 가동시키면 localhost:9090에 접속하면 Prometheus에 접속이 가능하다.</p>
<h4 id="결론">결론</h4>
<p>이 프로젝트를 통해 구축한 Prometheus 모니터링 시스템은 크롤링 작업의 안정성과 투명성을 제고하였다. 실시간 데이터 분석과 경고 시스템은 우리가 데이터 수집 작업에서 발생할 수 있는 문제를 빠르게 파악하고 해결할 수 있게 해준다. 앞으로도 이 시스템을 지속적으로 개선하여 더욱 정밀하고 효과적인 데이터 모니터링 환경을 구축해 나가겠다.</p>