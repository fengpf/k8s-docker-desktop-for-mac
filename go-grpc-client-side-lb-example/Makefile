build:
	cd greeter_client && GOOS=linux GOARCH=amd64 go build  . && cd ..
	cd greeter_server && GOOS=linux GOARCH=amd64 go build  . && cd ..
	docker build -t mountainfeng/greeter_test_k8s:0.1 .
	docker push mountainfeng/greeter_test_k8s:0.1
