## nginx域名重定向
```
if ($host != "www.hostname.com"){
   rewrite ^/(.*)$ http://www.hostname.com/$1 permanent;
}
```

