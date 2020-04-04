### 问题

使用 `brew install [软件包]` 安装软件时，卡在 `Updating Homebrew...`。

**原因**

在国内访问官方更新源获取资源太慢,解决方案可以采用更换国内镜像更新源。

**解决方案**

- 替换 `Homebrew` 源

```shell
$ cd "$(brew --repo)"
$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

- 替换 `homebrew-core` 源

```shell
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

- 替换 `homebrew-cask` 源

```shell
$ cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```

