# NGINX配置

配置DNS重定向到目标服务器。

## nginx反代GithubPage

> **参考**：[血衫非弧](https://blog.kelu.org/tech/2017/05/06/github-pages-reverse-proxy.html)

```txt
server {
        listen 80;
        server_name  域名;
        rewrite ^(.*)$ https://$host$request_uri permanent;    
}
server {
        listen 443 ssl http2;
        server_name  域名;
        location / 
        {
            proxy_pass         http://******.github.io;
            proxy_set_header   X-Host                        $host;
            proxy_set_header   X-Real-IP               $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header User-Agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36";
            #缓存
            proxy_cache_valid  200 206 304 301 302 1d; #对httpcode为200…的缓存1天 
            proxy_cache_valid  any 1d;    #其他的缓存多长时间  
            proxy_cache_key $uri;  #定义缓存唯一key,通过唯一key来进行hash存取
        }
        access_log off; #access_log end
        error_log off; #error_log end
}
```

配置说明：

```txt
proxy_set_header Host $host 设置请求头的Host为反向代理服务器的Host

proxy_set_header X-Real-IP $remote_addr 设置请求头的X-Real-IP为客户端真实IP

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for 把请求来源的IP添加到请求头的X-Forwarded-For字段

X-Forwarded-For:简称XFF头，它代表客户端，也就是HTTP的请求端真实的IP，只有在通过了HTTP代理或者负载均衡服务器时才会添加该项。 它不是RFC中定义的标准请求头信息，在squid缓存代理服务器开发文档中可以找到该项的详细介绍。 标准格式如下：X-Forwarded-For: client1, proxy1, proxy2。
```
