## nginx域名重定向
```
if ($host != "www.whaletrip.com"){
   rewrite ^/(.*)$ http://www.whaletrip.com/$1 permanent;
}
```
