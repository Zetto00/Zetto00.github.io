# 博客搭建笔记


本博客使用 GitHub Pages、Hugo 和 LoveIt，可以麻烦地定制化博客。

## 前期准备

### 安装 Hugo

详细的安装说明请参考 [官方文档](https://gohugo.io/getting-started/installing/)。注意安装 $> 0.62.0$ 的 extended 版本。

macOS 下可使用包管理工具 [Homebrew](https://brew.sh)[^Homebrew]快速安装：

```sh
brew install hugo
```

win10 下可使用包管理工具 [chocolatey](https://chocolatey.org/) 快速安装。以管理员权限打开 `cmd.exe`，~~没运行过，不保证这段代码的可行性~~

```sh
# 安装 chocolatey
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

# 安装 Hugo
choco install hugo-extended -y
```

验证 Hugo 是否安装成功：

```sh
hugo version
```

### 安装 Git

从 [官方网站](https://git-scm.com) 下载。或使用命令行：

```sh
brew install git
```

```sh
choco install git -y
```

## 附录 科学访问 GitHub

由于 DNS 污染[^DNS]和 SNI 封锁[^SNI] ，GitHub 间歇性可连接。使用项目 [dev-sidecar](https://github.com/docmirror/dev-sidecar) 可以科学访问 GitHub。其 [原理](https://github.com/docmirror/dev-sidecar/blob/master/doc/caroot.md) 是在本地启动代理服务器帮助访问目标网站；当目标网站需要拦截时（例如github），就需要通过中间人攻击修改请求或者请求其他替代网站，从而达到加速的目的。注意打开 `Git.exe代理` 选项。

## 附录 Homebrew 安装与加速

使用 `echo $0` 查看所使用的 shell。若为 bash 则将下文对应 `zsh` 改为 `bash`，`.zshrc` 改为 `.bash_profile`，

安装 Homebrew：

```sh
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

若还未安装 Git：

```sh
brew install git
```

使 Homebrew 从更快的源下载：

```sh
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
cd
brew update
```

Homebrew 默认从 Homebrew Bottles 源中下载二进制代码包安装。使 Homebrew Bottles 从更快的源下载：

```sh
# 使用 echo $0 查看所使用的 shell。若为 bash 则将 .zshrc 改为 .bash_profile
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

Homebrew Cask 可以下载常见的带界面的应用软件。

```sh
brew install cask
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
cd
homebrew update
```

验证 Homebrew 是否配置成功：

```sh
brew config
```



[^Homebrew]: 参见附录。
[^DNS]: Domain Name System，一个域名有多个服务器，域名解析至不正确的 IP。
[^SNI]: Server Name Indication，一个服务器有多个域名，目标域名明文传递。


