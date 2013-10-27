---
layout: post
title: '如何获得 Perl'
date: 2013-10-23 16:20
post-link: http://perl101.org/how-to-get-perl.html
---

这个系列文章基本上是对 Andy Lester 所撰写的 Perl 101 的翻译，
不过，我会根据自己的理解和认识对其进行扩充，同时，为了反映
Perl 当下的变化，我也会进行适当的改写。总之，希望这些内容能够
对 Perl 新手提供帮助。

既然你已经下定决心要学习 Perl 这门编程语言，那么摆在你面前的第
一件事就是得到它。

### 你有 Perl 吗

试试从命令行执行 `perl -v`，如果你看到 Perl 的版本及版权等信息，
那么说明你的系统已经具有 Perl。反之，如果你看到的是类似 `command not found`
这样的输出，那么你需要安装 Perl。

```
$ perl -v

This is perl 5, version 18, subversion 1 (v5.18.1) built for i486-linux-gnu-thread-multi-64int
(with 46 registered patches, see perl -V for more detail)

Copyright 1987-2013, Larry Wall

Perl may be copied only under the terms of either the Artistic License or the
GNU General Public License, which may be found in the Perl 5 source kit.

Complete documentation for Perl, including FAQ lists, should be found on
this system using "man perl" or "perldoc perl".  If you have access to the
Internet, point your browser at http://www.perl.org/, the Perl Home Page.
```

### GNU/Linux

Perl 支持许多平台，在 GNU/Linux 上基本都默认带有 Perl。但十有
八九可能是旧版本。这种情况下，你可以通过所用 GNU/Linux 发行版
的包管理器来更新 Perl。

### Mac OS X

Mac OS X 系统本身也默认安装了 Perl，不过可能仍然存在版本过旧的问题。为此，你可
以自己安装更新版。

### Windows

Windows 系统默认没有 Perl。你可以选择下列 Perl 发行之一：

1. [Strawberry](http://strawberryperl.com)：称为草莓 Perl，它专为 Windows
   平台而生，其中打包了 CPAN 客户端、编译器、以及预装了大量模块。除非你有
   很特殊的需求，一般来说这就是你所需要的 Perl 发行。

2. [ActiveState](http://www.activestate.com/activeperl)：Perl 针对 Windows
   平台的发起者，至今仍然活跃参与社区。ActiveState 发布自己打包的 Perl，并且
   包含 PPM 模块安装系统。如果你嫌麻烦，不想自己管理 Perl 安装，那么它也许适
   合你。

### Perl 源代码

Perl 源代码位于 <http://www.cpan.org/src/>。如果你打算自行编译安装 Perl，需要
准备编译器、Shell、以及某些系统库。如果你缺少某些东东，Perl 的 `Configure`
脚本将告诉你。通过以下指令可以从源代码编译并安装 Perl：

```
$ wget http://www.cpan.org/src/5.0/perl-5.18.1.tar.gz
$ tar -xzf perl-5.18.1.tar.gz
$ cd perl-5.18.1
$ ./Configure -des -Dprefix=$HOME/localperl
$ make
$ make test
$ make install
```

### Perlbrew 和 Plenv

除了手动从源代码编译、安装 Perl 之外，你也可以选用时下比较流行的 Perl
多版本管理工具 [Perlbrew][b] 或 [Plenv][e]。

#### Perlbrew

要安装 Perlbrew，你可以在终端中执行：

    $ curl -L http://install.perlbrew.pl | bash

然后，将下列内容添加到 `.bashrc` 或 `.zshrc` 文件中：

    source ~/perl5/perlbrew/etc/bashrc

接着执行：

    $ source ~/.bashrc
    $ source ~/.zshrc

至此，你便能够使用 Perlbrew 来安装 Perl 的各种版本了。

先列出可用的 Perl 版本：

    $ perlbrew available

安装具体的 Perl 版本：

    $ perlbrew install 5.18.1

待安装完毕，你可以通过以下指令来切换到刚安装的 Perl 版本：

    $ perlbrew switch perl-5.18.1

此外，Perlbrew 还有列出已安装的 Perl 版本、暂时关闭自身等功能，具体可以
查看其帮助文档。

#### Plenv

Plenv 的功能与 Perlbrew 类似，其安装步骤为：

    $ git clone git://github.com/tokuhirom/plenv.git ~/.plenv
    $ echo 'export PATH="$HOME/.plenv/bin:$PATH"' >> ~/.bash_profile
    $ echo 'eval "$(plenv init -)"' >> ~/.bash_profile
    $ exec $SHELL -l
    $ git clone git://github.com/tokuhirom/Perl-Build.git ~/.plenv/plugins/perl-build/

注意：Zsh 用户需将上述指令中的 `.bash_profile` 替换为 `.zshrc`。另外，Ubuntu
用户需将其替换成 `.profile`。

现在，你可以使用 Plenv 来安装 Perl：

    $ plenv install 5.18.1

安装完成后需要执行 `plenv rehash` 重建 shim 可执行文件。

Plenv 能够将某个 Perl 版本设置成局部、全局及 Shell 作用环境。其命令分别为：

    $ plenv local 5.18.1  # 设置为局部作用环境，比全局作用环境具有更高的优先级
    $ plenv global 5.18.1 # 设置成全局作用环境，将在所有 Shell 中使用
    $ plenv shell 5.18.1  # 设置成 Shell 作用环境，具有最高的优先级

关于 Plenv 的更多用法，可以通过 `plenv help` 查阅。

[b]: http://perlbrew.pl
[e]: https://github.com/tokuhirom/plenv
