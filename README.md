fix docker run host post: docker run -v D:\Docker\node_docker:/usr/src/app -p 80:80 -p 3000:80 -d --name node-app node-app-image

initals infor: -p 80:80 => "80" fisrt post is hot port in browser
                        => ":80" after first post is hot port in container post docker

docker build image: docker build -t image_name .

if node_modules does not exits: add a command: -v /app/node_modules when you run docker container

Fix: used: Ngnix


# Important Command In Docker:

1. docker build -t image_name .
2. docker run image_name
3. docker ps || docker ps -a
4. docker stop image_name #stop container image 
5. docker container prune #remove all container images
6. docker rm (image_name || id) --force #use --force when container is running

# Install jenkins with docker: *https://hub.docker.com/r/jenkins/jenkins*
# Link github: *https://github.com/jenkinsci/docker/blob/master/README.md*
1. docker volume create jenkins-home
2. docker run -d -p 9090:8080 -v jenkins-home:/var/jenkins_home ---restart=on-failure jenkins/jenkins:lts-jdk17
3. Unlock Jenkins: docker exec "CONTAINER ID" cat /var/jenkins_home/secrets/initialAdminPassword

# Build Steps
*df -kh*
*ls -lrth /var/jenkins_home*

Bước 1: Cài đặt Docker Compose
Bạn cần cài đặt Docker Compose trên máy tính của mình. Bạn có thể tải từ trang chủ của Docker hoặc có thể cài đặt thông qua các gói phần mềm của hệ điều hành.

Tiếp đến, bạn sẽ tạo Dockerfile để xác định environment. Mỗi dịch vụ sẽ chạy trên một container riêng và sử dụng image tương ứng.

Bước 2: Tạo file docker-compose.yml
Bạn cần tạo một file docker-compose.yml để định nghĩa các container và cấu hình của chúng. Ví dụ, tệp sẽ bao gồm các nội dung sau:


version: '3'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./web:/usr/share/nginx/html
    networks:
      - webnet
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
      POSTGRES_DB: example
    volumes:
      - dbdata:/var/lib/postgresql/data
    networks:
      - webnet

networks:
  webnet:

volumes:
  dbdata:

# Ý nghĩa của các giá trị trong file docker-compose:
1. version: phiên bản của Docker Compose file. Ở đây, chúng ta sử dụng phiên bản 3.
2. services: là khu vực khai báo các services cần thiết cho ứng dụng.
3. web: dịch vụ web, sử dụng image nginx, chia sẻ volume và network với dịch vụ db. Cổng 80 được định nghĩa để web có thể truy cập được từ bên ngoài.
4. db: dịch vụ db, sử dụng image postgresql, chia sẻ volume và network với dịch vụ web. Các biến môi trường cài đặt cho dịch vụ postgresql được định nghĩa ở phần environment.
5. networks: danh sách các networks được sử dụng cho container.
6. webnet: mạng webnet để chia sẻ giữa dịch vụ web và db.
7. volumes: là option nên config, volumes cho phép mount data từ container ra máy local. Khi config option này thì mỗi lần stop container data của container đó sẽ không bị mất đi.
8. dbdata: là container chứa thông tin về database.
Trong ví dụ này, Docker Compose sẽ khởi tạo 2 container, một container sử dụng image nginx và một container sử dụng image postgresql. Container sử dụng image nginx sẽ được kết nối với container sử dụng image postgresql thông qua mạng webnet. Sau đó, chúng sẽ chia sẻ volume dbdata để lưu trữ dữ liệu của postgresql.

Bước 3: Chạy lệnh docker-compose up
Sử dụng lệnh docker-compose up để khởi động các container được định nghĩa trong file docker-compose.yml. Nếu các image không được tải xuống trước đó, Docker Compose sẽ tự động tải chúng xuống và khởi động các container.