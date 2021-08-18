最近在公司windows服务器部署nginx前端项目时 因为业务需求 有个有个接口数据量很大，请求时长在很大可能超过一分钟 然后一直遇到了504 Gateway Time-out 在网上查了很多资料都是加

```nginx
    proxy_connect_timeout 1; 
    proxy_send_timeout 300; 
    proxy_read_timeout 300;                 
```

但是我加上之后没有用，头疼了一天 看日志upstream timed out (10060: A connection 这个错 然后百度也是说设置超时参数

最后怀疑是否是版本问题，之前用的的nginx-1.12.2,在官网下了个nginx-1.14.2，命令taskkill /im nginx.exe /f 杀死所有nginx进程 然后在配置里面加

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```nginx
            proxy_connect_timeout 1; 
            proxy_send_timeout 300; 
            proxy_read_timeout 300; 
            proxy_buffer_size 1M; 
            proxy_buffers 8 1M; 
            proxy_busy_buffers_size 1M; 
            proxy_temp_file_write_size 1M;
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

因为一般查询慢的接口很有可能返回的数据量很大128k,256k都无法满足 索性调到1M以防万一

最后重启nginx-1.14.2，问题解决