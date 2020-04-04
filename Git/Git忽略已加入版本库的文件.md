项目中，我们会用到 `.gitignore` 来忽略一些文件，不记录这些文件的版本控制。

然而经常发现，已经添加到 `.gitignore` 的文件/目录，每次的修改仍会记录版本。

原因是因为在添加 `.gitignore` 之前，这些文件已经添加到版本库了，那么如何忽略已加入版本库的文件？

**解决方法：**

```shell
git rm --cached file
git commit -m "delete remote file"
git push
```

