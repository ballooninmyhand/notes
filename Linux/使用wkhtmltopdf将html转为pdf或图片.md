### 使用 wkhtmltopdf 将 html 文件转为 pdf 或图片

**1. 安装 wkhtmltopdf**

- 通过 `https://wkhtmltopdf.org/downloads.html` 选择合适版本下载
- 解压 

> tar -xvf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz

- 拷贝程序到 `/usr/bin` 目录下

> cp wkhtmltox/bin/wkhtmltopdf /usr/bin/
>
> cp wkhtmltox/bin/wkhtmltoimage /usr/bin/



**2. 使用**

```shell
# 将 test.html 转为 pdf 文件
wkhtmltopdf test.html test.pdf

# 转为 png 文件，并设置图片质量为30，默认94
wkhtmltoimage --quality 30 test.html test.png 
```



**3. 中文乱码或空白问题**

- 问题：转出的图片或 pdf 存在中文乱码或者中文空白的问题

- 原因：`wkhtmltopdf` 在转换文件格式时，中文会默认使用宋体，但是服务器上缺少字体文件，所以显示乱码或者空白
- 解决方案：去网上下载 `simsun.ttc` 字体文件，拷贝到服务器的 `/usr/share/fonts/` 目录下，重新生成 pdf 或图片，中文显示正常