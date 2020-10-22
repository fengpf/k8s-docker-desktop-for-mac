### 创建服务

```
mkdir -p /Users/fpf/promethues
mkdir -p /Users/fpf/promethues/server
mkdir -p /Users/fpf/promethues/client
touch /Users/fpf/promethues/server/prometheus.yml
touch /Users/fpf/promethues/server/rules.yml
chmod 777 /Users/fpf/promethues/server/rules.yml
chmod 777 /Users/fpf/promethues/server/prometheus.yml


docker rm -f prometheus

docker run --name=prometheus -d \
-p 9090:9090 \
-v /Users/fpf/promethues/server/prometheus.yml:/etc/prometheus/prometheus.yml \
-v /Users/fpf/promethues/server/rules.yml:/etc/prometheus/rules.yml \
prom/prometheus:v2.7.2 \
--config.file=/etc/prometheus/prometheus.yml \
--web.enable-lifecycle

mkdir -p /Users/fpf/promethues/alertmanager

docker rm -f alertmanager

docker run -d -p 9093:9093 \
--name alertmanager \
-v /Users/fpf/promethues/alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml \
prom/alertmanager

```