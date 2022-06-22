# 安装
## 在 Linux 上安装

[Git (git-scm.com)](https://git-scm.com/download/linux)

## 在 macOS 上安装

[Git - Downloading Package (git-scm.com)](https://git-scm.com/download/mac)

## 在 Windows 上安装

[Git - Downloading Package (git-scm.com)](https://git-scm.com/download/win)

## 从源代码安装

如果你想从源码安装 Git，需要安装 Git 依赖的库：autotools、curl、zlib、openssl、expat 和 libiconv。 如果你的系统上有 `dnf` （如 Fedora）或者 `apt`（如基于 Debian 的系统）， 可以使用对应的命令来安装最少的依赖以便编译并安装 Git 的二进制版：
```console
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \
  openssl-devel perl-devel zlib-devel
$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \
  gettext libz-dev libssl-dev
```
为了添加文档的多种格式（doc、html、info），需要以下附加的依赖：
```console
$ sudo dnf install asciidoc xmlto docbook2X
$ sudo apt-get install asciidoc xmlto docbook2x
```

>使用 RHEL 和 RHEL 衍生版，如 CentOS 和 Scientific Linux 的用户需要 [开启 EPEL 库](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F) 以便下载 `docbook2X` 包。
>> CentOS
>> 检查是否开启：`sudo yum install epel-release`
>> 安装EPEL发行包（开启）：`sudo yum repolist`  

如果你使用基于 Debian 的发行版（Debian/Ubuntu/Ubuntu-derivatives），你也需要 `install-info` 包：
```console
$ sudo apt-get install install-info
```
如果你使用基于 RPM 的发行版（Fedora/RHEL/RHEL衍生版），你还需要 `getopt` 包 （它已经在基于 Debian 的发行版中预装了）：
```console
$ sudo dnf install getopt
```
此外，如果你使用 Fedora/RHEL/RHEL衍生版，那么你需要执行以下命令：
```console
$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
```
以此来解决二进制文件名的不同。

当你安装好所有的必要依赖，你可以继续从几个地方来取得最新发布版本的 tar 包。 你可以从 Kernel.org 网站获取，网址为 [https://www.kernel.org/pub/software/scm/git](https://www.kernel.org/pub/software/scm/git)， 或从 GitHub 网站上的镜像来获得，网址为 [https://github.com/git/git/releases](https://github.com/git/git/releases)。 通常在 GitHub 上的是最新版本，但 kernel.org 上包含有文件下载签名，如果你想验证下载正确性的话会用到。
接着，编译并安装：
```console
$ tar -zxf git-2.8.0.tar.gz
$ cd git-2.8.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```
完成后，你可以使用 Git 来获取 Git 的更新：
```console
$ git clone git://git.kernel.org/pub/scm/git/git.git
```

# 初次运行 Git 前的配置

定制你的 Git 环境。 每台计算机上只需要配置一次，程序升级时会保留配置信息。

Git 自带一个 [`git config`](https://git-scm.com/docs/git-config) 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：
1.  `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 `git config` 时带上 `--system` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）    
2.  `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 你可以传递 `--global` 选项让 Git 读写此文件，这会对你系统上 **所有** 的仓库生效。
3.  当前使用仓库的 Git 目录中的 `config` 文件（即 `.git/config`）：针对该仓库。 你可以传递 `--local` 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）
每一个级别会覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。
你可以通过以下命令查看所有的配置以及它们所在的文件：
```console
$ git config --list --show-origin
```
通过输入 `git config <key>`： 来检查 Git 的某一项配置
```console
$ git config user.name
John Doe
```
```console
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig	false
```

## 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```console
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

很多 GUI 工具都会在第一次运行时帮助你配置这些信息。

## 文本编辑器

配置默认文本编辑器了，当 Git 需要你输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑器。
如果你想使用不同的文本编辑器，例如 Emacs，可以这样做：
```console
$ git config --global core.editor emacs
```
在 Windows 系统上，如果你想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。 它可能随你的编辑器的打包方式而不同。

如果你在使用其他的或 32 位版本的编辑器，请在 [`core.editor`](https://git-scm.com/book/zh/v2/ch00/_core_editor) 中查看设置为该编辑器的具体步骤。
>如果你不这样设置编辑器，那么当 Git 试图启动它时你可能会被弄糊涂、不知所措。 例如，在 Windows 上 Git 在开始编辑时可能会过早地结束。

# 离线获取帮助

若你使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：
```console
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```
例如，要想获得 `git config` 命令的手册，执行
```console
$ git help config
```

# Git 命令

`git init` 在已存在目录中初始化仓库
`git clone` 克隆现有的仓库
Git全指令参考：https://git-scm.com/docs

# 常用操作

- 获取 - 拉取 - 提交 - 推送 - [合并]
- 可以在任一提交上创建新分支，推送是把指定的本地分支 同步到 指定的服务器分支
- 提交后没有推送，可以重置分支来删除提交（可保持本地所有变动）
- 推送后可以回滚提交，恢复到之前的提交
- 两分支没有更改冲突可以合并
- 提交代码时，用户的验证方式
  >账号密码验证
  >SSH客户端配置：本地使用ssh命令创建rsa密钥，然后在服务器设置ssh公钥
- 创建`.gitignore`文件忽略不提交的文件
  >HELP.md
  >target/
  >!.mvn/wrapper/maven-wrapper.jar
  >!\*\*/src/main/\*\*/target/
  >!\*\*/src/test/\*\*/target/
- 贡献别人的项目：[Fork](https://git-scm.com/book/zh/v2/GitHub-%E5%AF%B9%E9%A1%B9%E7%9B%AE%E5%81%9A%E5%87%BA%E8%B4%A1%E7%8C%AE)

# 相关链接

Git官网：https://git-scm.com/
Git开源地址：https://github.com/git/git
从零开始：https://git-scm.com/book/zh/v2
Gitee：https://gitee.com/
Github：https://github.com/
Sourcetree：https://www.sourcetreeapp.com/