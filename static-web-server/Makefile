build:
	mkdir -p build && \
	cd build && \
	cmake .. && \
	make -j4 

clean:
	rm -rf build

rebuild: clean build

dev-deps:
	sudo apt -y -q install cppcheck
	sudo apt -y -q install python3-pip
	sudo apt -y -q install cmake
	sudo pip3 install cpplint

run-nginx:
	docker run -d --rm -p 8888:8888 --name nginx -t nginx

build-nginx-docker:
	docker build -t nginx ./nginx-test

build-server-docker:
	docker build -t static_server .

run-server:
	docker run --name static_server -p 80:80 -d static_server

bench-static-server:
	ab -n 1000 -c 100 http://localhost/httptest/wikipedia_russia.html

bench-nginx:
	ab -n 1000 -c 100 http://localhost:8888/httptest/wikipedia_russia.html

check:
	cppcheck --enable=all --language=c */*.c */*/*.h
	cpplint --extensions=c */*.c

.PHONY: build