# Nginx

## 查询nginx的位置
whereis Nginx

## 判断Nignx的服务是否启动
ps aux | grep nginx

## 判断80端口被谁占用
netstat -tulnp | grep :80

## 关闭
kill -9 nginx

./nginx -s quit  等待niginx的工作做完才关闭
./nginx -s stop  直接关闭


## 配置Nginx的反向代理
反向代理： nginx等待客户的请求
正向代理： nginx主动发出去的请求

## 在http里面配置代理
upstream tomcat1{
  server 192.168.239.128：8080
}

## 在server里面配置代理拦截
proxy_pass http：//tomcat1；






