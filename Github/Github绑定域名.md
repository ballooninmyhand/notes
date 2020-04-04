## Github 绑定域名

本文讲述的是如何利用 Github 绑定自己的域名。



### 操作步骤

- 登录 github，添加新仓库，仓库名必须为 `用户名.github.io`，如下图：

![image-20200404181348082](https://tva1.sinaimg.cn/large/00831rSTgy1gdhwfq2zr3j31jk0b0q8h.jpg)

- 点击 `Settings`

![image-20200404181448909](https://tva1.sinaimg.cn/large/00831rSTgy1gdhwgsynghj31ju0aqaf4.jpg)

- 编辑 `Custom domain`，输入自己的域名，点击 `Save`

![image-20200404181548651](https://tva1.sinaimg.cn/large/00831rSTgy1gdhwht8yhbj30zq0u04bp.jpg)

- 登录阿里云，进入控制台——域名——解析，如下图：

![image-20200404181931408](https://tva1.sinaimg.cn/large/00831rSTgy1gdhwlovrxaj31xg09yadn.jpg)

- 添加两条 `CNAME` 记录，如下图：

![image-20200404182021045](https://tva1.sinaimg.cn/large/00831rSTgy1gdhwmlh7ymj31vc0iu10y.jpg)

至此，配置就全部完成了，只需要等待几分钟，就可以直接访问自己的域名了。