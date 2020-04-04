在使用 git 进行版本管理时，有时只是修改了文件的权限，比如将 `test.php` 权限改为 777，文件内容不做修改，但 git 会认为此文件做了修改，原因是 git 把文件权限也算作文件差异的一部分了。

那么如何忽略文件权限造成的文件差异呢？

**解决方法：**

修改项目中 `.git/config` 文件，将 `filemode` 改为 `false`。或者执行命令 `git config core.filemode false` 即可。