## nginx域名重定向

#### 添加配置

```shell
if ($host != "www.hostname.com"){
   rewrite ^/(.*)$ http://www.hostname.com/$1 permanent;
}
```

