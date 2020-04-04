#### 问题1

项目中，我们会用到 `.gitignore` 来忽略一些文件，不记录这些文件的版本控制。

然而经常发现，已经添加到 `.gitignore` 的文件/目录，每次的修改仍会记录版本。

原因是因为在添加 `.gitignore` 之前，这些文件已经添加到版本库了，那么如何忽略已加入版本库的文件？

**解决方法：**

```shell
git rm --cached file
git commit -m "delete remote file"
git push
```



#### 问题2

在使用 git 进行版本管理时，有时只是修改了文件的权限，比如将 `test.php` 权限改为 777，文件内容不做修改，但 git 会认为此文件做了修改，原因是 git 把文件权限也算作文件差异的一部分了。

那么如何忽略文件权限造成的文件差异呢？

**解决方法：**

修改项目中 `.git/config` 文件，将 `filemode` 改为 `false`。或者执行命令 `git config core.filemode false` 即可。

