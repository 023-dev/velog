<h4 id="서론">서론</h4>
<p>우리의 웹 크롤링 자동화 프로젝트는 이제 Prometheus를 통한 로깅 및 모니터링 시스템을 성공적으로 구축하였다. 이를 한 단계 더 발전시켜, 실시간 경고 관리를 위해 Prometheus의 Alertmanager를 도입하고, Slack 웹훅과 연동하여 즉각적인 알림을 받을 수 있도록 하겠다. 이 글에서는 Alertmanager 설정과 Slack 연동 과정을 자세히 설명한다.</p>
<h4 id="alertmanager의-도입">Alertmanager의 도입</h4>
<p>Alertmanager는 Prometheus 에코시스템의 중요한 부분으로, 조건에 따라 경고를 발생시키고 이를 적절한 수신자에게 전달한다. 이를 통해 시스템 이슈가 발생했을 때 신속한 대응이 가능하다. Alertmanager는 다양한 경고 채널을 지원하며, 이번 프로젝트에서는 Slack을 주요 알림 수단으로 선택하였다.</p>
<h4 id="구현-과정">구현 과정</h4>
<ol>
<li><p><strong>Alertmanager 설정</strong></p>
<ul>
<li><strong>작업 설명</strong>: Alertmanager를 Prometheus와 통합하여 특정 조건을 만족하는 경고를 생성하겠다. 이를 위해 Alertmanager의 설정 파일을 작성하고, 경고 규칙을 정의하겠다.</li>
<li><strong>구현 방법</strong>:
```yaml
global:  #global: Alertmanager의 전역 설정입니다.
resolve_timeout: 5m  #resolve_timeout: 경고가 해결된 것으로 간주되기 전     대기 시간.
route:  #route: 경고를 처리하는 방법을 정의.
receiver: 'slack-notifications'  #receiver: 기본 수신자.
group_by: ['alertname']  #group_by: 경고를 그룹화할 기준.
group_wait: 30s  #group_wait: 경고를 그룹화하기 전 대기 시간.
group_interval: 5m  #group_interval: 그룹화된 경고를 다시 전송하기 전 대기 시간.
repeat_interval: 3m  #repeat_interval: 동일한 경고를 반복해서 전송하기 전 대기 시간.
receivers:  #receivers: 알림을 받을 수신자 목록.</li>
<li>name: 'slack'
slack_configs:  #slack_configs: Slack 알림 설정.<ul>
<li>api_url:   #api_url: Slack에서 생성한 Webhook URL.
channel: '#alerts'  #channel: 메시지를 보낼 Slack 채널.
send_resolved: true  #send_resolved: 경고가 해결되었을 때 알림을 보낼지 여부.<pre><code></code></pre></li>
</ul>
</li>
</ul>
</li>
<li><p><strong>Slack 웹훅 설정</strong></p>
<ul>
<li><strong>작업 설명</strong>: Slack 채널에 경고 메시지를 보낼 수 있도록 Slack 웹훅을 설정하겠다. 이를 통해 조직 내 또는 팀원들이 즉시 경고를 받고 필요한 조치를 취할 수 있다.</li>
<li><strong>구현 방법</strong>:<ul>
<li>Slack에서 적절한 채널을 생성하고, 웹훅 URL을 생성한다.</li>
<li>Alertmanager 설정 파일에 Slack 웹훅 URL을 포함시켜 경고 메시지가 해당 Slack 채널로 전송되도록 한다.</li>
</ul>
</li>
</ul>
</li>
<li><p><strong>경고 규칙 정의</strong></p>
<ul>
<li><p><strong>작업 설명</strong>: 크롤링 작업이나 시스템 성능에 문제가 발생했을 때 경고를 발생시키는 규칙을 예시 느낌으로 정의하겠다. </p>
</li>
<li><p><strong>구현 방법</strong>:
```yaml
groups:  #groups: 경고 규칙 그룹입니다.</p>
</li>
<li><p>name: prometheus  #name: 그룹 이름입니다.
rules:  #rules: 경고 규칙 목록입니다.</p>
<ul>
<li><p>alert: HighRequestLatency  #alert: 경고 이름입니다.
expr: job:request_latency_seconds:mean5m{job=&quot;CrawlerJob&quot;} &gt; 0.5  #expr: 경고 조건을 정의하는 PromQL 표현식입니다.
for: 10m  #for: 조건이 충족되어야 하는 지속 시간입니다.
labels:  #labels: 경고에 추가할 레이블입니다.
 severity: 'page'
annotations:  #annotations: 경고에 대한 추가 정보입니다.
 summary: &quot;High request latency&quot;
 description: &quot;Request latency is above 0.5s for more than 10 minutes.&quot;</p>
</li>
<li><p>alert: CrawlerStart
expr: up{job=&quot;crawler&quot;} == 1
for: 1m
labels:
 severity: info
annotations:
 summary: &quot;Crawler started&quot;
 description: &quot;The crawler has started.&quot;</p>
</li>
<li><p>alert: CrawlerSuccess
expr: increase(successful_crawls_total[1m]) &gt; 0
for: 1m
labels:
 severity: info
annotations:
 summary: &quot;Crawler succeeded&quot;
 description: &quot;The crawler has successfully fetched movie data.&quot;</p>
</li>
<li><p>alert: CrawlerDown
expr: absent(crawler_status{job=&quot;CrawlerJob&quot;} == 1)
for: 1m
labels:
 severity: critical
annotations:
 summary: &quot;Crawler is down&quot;
 description: &quot;The crawler has been down for more than 1 minute.&quot;</p>
</li>
<li><p>alert: HighErrorRate
expr: increase(failed_crawls_total[1m]) &gt; 5  # 1분간 실패한 크롤링 횟수가 5회를 초과할 때
for: 1m
labels:
 severity: critical
annotations:
 summary: &quot;High error rate in crawling&quot;
 description: &quot;Failed crawl rate is {{ $value }} failures per minute.&quot;</p>
</li>
<li><p>alert: NoMoviesFetched
expr: increase(requests_total[5m]) &gt; 0 and increase(successful_crawls_total[5m]) == 0  # 5분간 요청이 있었지만 성공적인 크롤링이 없을 때
for: 5m
labels:
 severity: warning
annotations:
 summary: &quot;No movies fetched&quot;
 description: &quot;Requests were made but no movies were fetched in the last 5 minutes.&quot;</p>
<pre><code></code></pre></li>
</ul>
</li>
</ul>
</li>
</ol>
<h4 id="결론">결론</h4>
<p>Alertmanager의 도입으로 우리의 크롤링 시스템은 이제 실시간 경고 기능을 갖추게 되었다. 이 시스템을 통해 발생 가능한 문제를 즉시 감지하고 빠르게 대응할 수 있을 것이다. </p>