## nginx+docker部署流程
### 1.准备工作
#### 1.1 前端打包
- src\axios.js
```javascript
axios.defaults.baseURL = "http://localhost:8081"
```
- vueblog-vue/vue.config.js
```javascript
module.exports = {
publicPath: '/'
}
```
#### 1.2 后端打包
- application.yml
默认配置文件
```yml
mybatis-plus:
  mapper-locations: classpath*:/mapper/**Mapper.xml
server:
  port: 8081
anyi:
  jwt:
    secret: f4e2e52034348f86b67cde581c0f9eb5
    expire: 604800
    header: Authorization
```
- application-default.yml
本地配置文件
```yml
# DataSource Config
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/vueblog?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: 196174
shiro-redis:
  enabled: true
  redis-manager:
    host: 127.0.0.1:6379
```
- application-pro.yml
部署到服务器上的配置文件
```yml
# DataSource Config
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://mysql:3306/vueblog?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: admin
shiro-redis:
  enabled: true
  redis-manager:
    host: redis:6379
```
- 执行打包命令：
```sh
mvn clean package -Dmaven.test.skip=true
```
- 得到target下的vueblog-0.0.1-SNAPSHOT.jar，然后再执行命令
```sh
java -jar vueblog-0.0.1-SNAPSHOT.jar --spring.profiles.active=default
```
### 2. Linux环境安装
2.1 安装docker 
```sh
#安装
yum install docker
#检验安装是否成功
[root@localhost opt]# docker --version
Docker version 1.13.1, build 7f2769b/1.13.1
#启动
systemctl start docker
```
2.2 安装docker-compose
```sh
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
# 授权
sudo chmod +x /usr/local/bin/docker-compose
# 检查是否安装成功
docker-compose --version
```
2.3 编写Dockerfile文件
- Dockerfile
```file
FROM java:8
EXPOSE 8080
ADD vueblog-0.0.1-SNAPSHOT.jar app.jar
RUN bash -c 'touch /app.jar'
ENTRYPOINT ["java", "-jar", "/app.jar", "--spring.profiles.active=pro"]
```
2.4、编写docker-compose.yml文件
- docker-compose.yml
```yml
version: "3"
services:
nginx: # 服务名称，用户自定义
  image: nginx:latest  # 镜像版本
  ports:
  - 80:80  # 暴露端口
  volumes: # 挂载
  - /root/nginx/html:/usr/share/nginx/html
  - /root/nginx/nginx.conf:/etc/nginx/nginx.conf
  privileged: true # 这个必须要，解决nginx的文件调用的权限问题
mysql:
  image: mysql:5.7.27
  ports:
  - 3306:3306
  environment: # 指定用户root的密码
    - MYSQL_ROOT_PASSWORD=admin
redis:
  image: redis:latest
vueblog:
  image: vueblog:latest
  build: . # 表示以当前目录下的Dockerfile开始构建镜像
  ports:
  - 8081:8081
  depends_on: # 依赖与mysql、redis，其实可以不填，默认已经表示可以
    - mysql
    - redis
```
2.4、准备好nginx的挂载目录和配置
- 宿主机的挂载目录：/root/nginx/html
- 挂载配置：/root/nginx/nginx.conf
- nginx.conf
```file
#user  root;
worker_processes  1;
events {
  worker_connections  1024;
}
http {
  include       mime.types;
  default_type  application/octet-stream;
  sendfile        on;
  keepalive_timeout  65;
  server {
      listen       80;
      server_name  localhost;
      location / {
          root   /usr/share/nginx/html;
          try_files $uri $uri/ /index.html last; # 别忘了这个哈
          index  index.html index.htm;
      }
      error_page   500 502 503 504  /50x.html;
      location = /50x.html {
          root   html;
      }
  }
}
```
**2.5 上传文件执行部署**
1. ==jar、DockerFile、docker-compose.yml== 上传到 ==root==目录下
![Alt text](http://www.ease.center/images/blog3.png)
2. 前端 ==dist==目录下的文件上传到==html==目录下
3. ![Alt text](http://www.ease.center/images/blog1.png)
3. ==nginx.config== 上传到 和 ==html== 同一级目录下
![Alt text](http://www.ease.center/images/blog2.png)

**2.6 执行部署命令**
```bash
docker-compose up -d
```

**2.7 docke一键命令**

```bash
一键启动所有docker 容器：
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
一键关闭所有docker 容器：
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
一键删除所有docker 容器：
docker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)
一键删除所有docker 镜像: 
docker rmi $(docker images | awk '{print $3}' |tail -n +2)
```



