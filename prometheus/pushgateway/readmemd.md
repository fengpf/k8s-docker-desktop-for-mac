
mkdir /Users/fpf/promethues/pushgateway

docker run -d -p 9091:9091 --name pushgateway prom/pushgateway


cat <<EOF | curl --data-binary @- http://10.23.9.38/:9091/metrics/job/cqh/instance/test
# 锻炼场所价格
muscle_metric{label="gym"} 8800
# 三大项数据 kg
bench_press 100
dead_lift 160
deep_squal 160
EOF