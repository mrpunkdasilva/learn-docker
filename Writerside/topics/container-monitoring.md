# Container Monitoring: Os Olhos do Sistema 👁️

```ascii
╔══════════════════════════════════════════════╗
║ MONITORING MATRIX                            ║
║                                              ║
║    ┌─────────┐    ┌──────────┐   ┌────────┐ ║
║    │Métricas │───▶│ Alerting │──▶│ Action │ ║
║    └─────────┘    └──────────┘   └────────┘ ║
║         │              ▲             ▲       ║
║         ▼              │             │       ║
║    ┌─────────┐    ┌──────────┐   ┌────────┐ ║
║    │ Logging │───▶│ Analysis │──▶│ Report │ ║
║    └─────────┘    └──────────┘   └────────┘ ║
╚══════════════════════════════════════════════╝
```

## Métricas Essenciais 📊

### Container Health
```bash
# CPU Usage
docker stats --format "table {{.Container}}\t{{.CPUPerc}}"

# Memory Consumption
docker stats --format "table {{.Container}}\t{{.MemUsage}}"

# I/O Operations
docker stats --format "table {{.Container}}\t{{.BlockIO}}"

# Detailed Stats
docker stats --no-stream --format \
  "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}\t{{.BlockIO}}"
```

### Advanced Metrics Collection

#### cAdvisor Integration
```yaml
version: '3'
services:
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:v0.47.0
    container_name: cadvisor
    privileged: true
    devices:
      - /dev/kmsg:/dev/kmsg
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - "8080:8080"
```

#### Node Exporter Setup
```yaml
version: '3'
services:
  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    command:
      - '--path.procfs=/host/proc'
      - '--path.rootfs=/rootfs'
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    ports:
      - "9100:9100"
```

## Observability Stack 🔭

### Prometheus Advanced Configuration
```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alert.rules"

scrape_configs:
  - job_name: 'containers'
    static_configs:
      - targets: ['localhost:9090']
    metrics_path: '/metrics'
    relabel_configs:
      - source_labels: [__meta_docker_container_name]
        target_label: container_name
      - source_labels: [__meta_docker_container_label_com_docker_compose_service]
        target_label: service_name

  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

### Grafana Dashboards

#### Container Overview
```json
{
  "dashboard": {
    "panels": [
      {
        "title": "CPU Usage",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(container_cpu_usage_seconds_total{name=~\"$container\"}[1m])",
            "legendFormat": "{{name}}"
          }
        ]
      },
      {
        "title": "Memory Usage",
        "type": "graph",
        "targets": [
          {
            "expr": "container_memory_usage_bytes{name=~\"$container\"}",
            "legendFormat": "{{name}}"
          }
        ]
      }
    ]
  }
}
```

## Advanced Logging Architecture 📝

### Fluentd Configuration
```xml
<source>
  @type forward
  port 24224
  bind 0.0.0.0
</source>

<filter docker.**>
  @type parser
  format json
  key_name log
  reserve_data true
</filter>

<match docker.**>
  @type elasticsearch
  host elasticsearch
  port 9200
  logstash_format true
  logstash_prefix docker
  include_tag_key true
</match>
```

### Vector Pipeline
```toml
[sources.docker_logs]
type = "docker_logs"
include_containers = ["app*", "web*"]

[transforms.parse_json]
type = "remap"
inputs = ["docker_logs"]
source = '''
. = parse_json!(.message)
'''

[sinks.elasticsearch]
type = "elasticsearch"
inputs = ["parse_json"]
endpoint = "http://elasticsearch:9200"
index = "logs-%Y-%m-%d"
```

## Distributed Tracing 🕸️

### Jaeger Configuration
```yaml
version: '3'
services:
  jaeger:
    image: jaegertracing/all-in-one:latest
    ports:
      - "5775:5775/udp"
      - "6831:6831/udp"
      - "6832:6832/udp"
      - "5778:5778"
      - "16686:16686"
      - "14250:14250"
      - "14268:14268"
      - "14269:14269"
      - "9411:9411"
```

### OpenTelemetry Integration
```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318

processors:
  batch:
    timeout: 1s
    send_batch_size: 1024

exporters:
  jaeger:
    endpoint: jaeger:14250
    tls:
      insecure: true

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [jaeger]
```

## Advanced Alerting Strategies 🚨

### Alert Manager Configuration
```yaml
global:
  resolve_timeout: 5m
  slack_api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'

route:
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  receiver: 'slack-notifications'

receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '#monitoring'
    title: "{{ range .Alerts }}{{ .Annotations.summary }}\n{{ end }}"
    text: "{{ range .Alerts }}{{ .Annotations.description }}\n{{ end }}"
```

### PagerDuty Integration
```yaml
receivers:
- name: 'pagerduty-critical'
  pagerduty_configs:
  - service_key: '<your-pagerduty-key>'
    severity: 'critical'
    description: '{{ template "pagerduty.default.description" . }}'
    client: 'monitoring-system'
    client_url: '{{ template "pagerduty.default.clientURL" . }}'
```

## Performance Optimization 🚀

### Resource Quotas
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```

### Monitoring Best Practices

#### Retention Policies
```sql
CREATE RETENTION POLICY "one_month"
ON "docker_metrics"
DURATION 30d
REPLICATION 1
DEFAULT
```

#### Downsampling
```sql
SELECT mean("value")
FROM "cpu_usage"
WHERE time > now() - 7d
GROUP BY time(1h)
```

## Troubleshooting Guide 🔍

### Common Scenarios Matrix
| Cenário | Sintomas | Métricas Chave | Ação Corretiva |
|---------|----------|----------------|----------------|
| Memory Leak | Crescimento constante de memória | container_memory_usage_bytes | Heap dump e análise |
| Network Bottleneck | Latência alta | container_network_transmit_bytes_total | Network trace |
| Disk I/O | Tempo de resposta alto | container_fs_writes_bytes_total | iostat análise |
| CPU Throttling | Performance degradada | container_cpu_cfs_throttled_seconds_total | Profile CPU |

### Debug Toolkit
```bash
# Container Process Tree
pstree -p $(docker inspect -f '{{.State.Pid}}' container_id)

# Network Connections
nsenter -t $(docker inspect -f '{{.State.Pid}}' container_id) -n netstat -tupn

# File System Usage
docker run --rm -v /:/host:ro alpine:latest df -h /host

# Stack Trace
docker exec container_id jstack $(pgrep java)
```

> **Monitor-chan diz:** "Métricas são como batimentos cardíacos do sistema - monitore-os ou enfrente o flatline! uwu"
{style="tip"}

> **Alert-sama avisa:** "Um alerta ignorado hoje é um incidente crítico amanhã! Nya~"
{style="warning"}

> **Trace-senpai sugere:** "Distributed tracing é como seguir pegadas digitais no cyberspaço! owo"
{style="note"}
