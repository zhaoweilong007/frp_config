# frp配置

> 折腾frp的记录，记录下frp的配置,
> frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力

### 服务器配置frps,ini

~~~properties
[common]
#绑定端口
bind_port=7000
#http需要设置代理端口
vhost_http_port=80
#开启token认证
authentication_method=token
token=xxxx
#管理后台配置
dashboard_addr=0.0.0.0
dashboard_port=7500
dashboard_user=xxxx
dashboard_pwd=xxxx
enable_prometheus=true
//日志文件
log_file=./frps.log
log_level=info
log_max_days=3
~~~

执行命令

```shell
bash -c ./frps -c frps.ini
```

### 客户端配置frpsc.ini

~~~properties
[common]
#服务器配置
server_addr=xx.xx.xx.xx
server_port=xxxx
token=xxxx
#[ssh]
#type = tcp
#local_ip = 127.0.0.1
#local_port = 22
#remote_port = 6000
#web配置
[web_a]
type=http
local_port=8080
#域名或者使用subdomain子域名
custom_domains=xxx.xxx.xxx
~~~

执行命令

```shell
bash -c ./frpc -c frpc.ini
```

### 配置Nginx代理

```
server {
    listen 80;
    server_name abc.com;

	# frp管理后台
    location / {
        proxy_pass http://127.0.0.1:7500;
        proxy_set_header    Host            $host:80;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_hide_header   X-Powered-By;
    }
}

#配置frp代理
server {
    listen       80;
    server_name  frp.abc.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header    Host            $host:80;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_hide_header   X-Powered-By;
    }
}
```