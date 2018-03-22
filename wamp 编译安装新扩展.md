### 以 memcache 为例

1. 安装 memcached

   参照 <http://www.runoob.com/memcached/window-install-memcached.html> 即可

2. 安装 memcache 扩展

- 根据PHP版本下载相应的memcache安装包（<http://windows.php.net/downloads/pecl/releases/memcache/3.0.8/> ）；
- 解压下载的安装包，将php_memcache.dll和php_memcache.pdb文件复制到php所安装的ext目录下；
- 打开php.ini文件，添加一句：extension=php_memcache.dll；
- 重启apache服务；
- 编写测试代码如下：

```php
<?php
	$mem = new Memcache;
	$mem->addServer('localhost',11211);
	$mem->set('key','I am memcache!');
	$mem->get('key');
	echo $mem->get('key');
```

- 浏览器访问该文件，打印输出：I am memcache!