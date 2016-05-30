test: clean build
	docker run -d -p 32001:22 --name chage-centos5 chage-centos5
	docker run -d -p 32002:22 --name chage-ubuntu14.04 chage-ubuntu14.04
	ansible-playbook -i hosts test.yml

build:
	mkdir -p ./tmp
	cp ~/.ssh/id_dsa.pub ./tmp
	docker build -t chage-centos5 -f Dockerfile.centos5 .
	docker build -t chage-ubuntu14.04 -f Dockerfile.ubuntu14.04 .
	rm ./tmp/id_dsa.pub
	rmdir ./tmp/

clean:
	-docker rm -f chage-centos5
	-docker rm -f chage-ubuntu14.04
	rm -f test.retry
