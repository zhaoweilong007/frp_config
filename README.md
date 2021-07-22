# frp配置

>折腾frp的记录，记录下frp的配置,
>frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力

### 服务器配置frps,ini

```shell
bash -c ./frps -c frps.ini
```

### 客户端配置frpsc.ini
```shell
bash -c ./frpc -c frpc.ini
```

### 配置Nginx代理