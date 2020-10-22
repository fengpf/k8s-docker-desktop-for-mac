
```
go build -o random random.go

./random -listen-address=:8080 &
./random -listen-address=:8081 &
./random -listen-address=:8082 &

docker run -d \
--name=node-exporter \
-p 9100:9100 \
prom/node-exporter

```

