---
layout: post
title: 命令行选项
date: 2013-10-28 15:14
post-link: http://perl101.org/command-line-switches.html
---

### Shebang 行

几乎每个 Perl 程序都如此开始：

```perl
#!/usr/bin/perl
```

这是 UNIX 结构，它告诉 Shell 直接执行余下的输入程序文件。

你可以在此行添加 Perl 的任何命令行选项，它们将成为选项之后命令行的一部分。
如果你有一个程序包含：

```perl
#!/usr/bin/perl -T
```

然后执行：

```
perl -l program.pl
```

`-l` 和 `-T` 两个选项都会使用，但 `-l` 将先用。在 *perlrun* 文档中介绍
了 Perl 的命令行选项。此处只介绍最有用的内容。

### perl -T

Perl 允许你在 `taint` 模式执行。在此模式中，变量在使用前需要“消毒”，以
应对不安全的操作。

何为不安全？

* 执行程序
* 写入文件
* 创建目录
* 基本上，修改系统的任何事情

如果你没有“去污”数据，那么这些操作将是程序中的严重错误。

如何去污？使用正则表达式匹配有效的值，然后将匹配赋给变量。

```perl
my ($ok_filename) = $filename =~ /^(\w+\.log)$/;
```

你应当达到程序 `taint` 安全的目的。

### perl -c file.{pl,pm}

此命令行选项允许检查给定文件的语法错误。它也会执行 `BEGIN` 块中的任意
代码，并检查程序中已使用的模块。

你应当使用 `-c` 在每次更改后检查代码的语法。

### perl -e 'code'

该选项允许你从命令行执行代码，以代替将程序写入文件来执行。

```perl
$ perl -e 'print "1\n"'
1
```

这对小程序、快速计算、以及与其他选项组合使用非常有用。

### -n、-p、-i

Perl 的 `-n` 选项允许你针对标准输入的每行重复执行代码（通常使用 `-e` 指定）。
这些是等效的：

```
$ cat /etc/passwd | perl -e 'while (<>) { if (/^(\w+):/) { print "$1\n"; } }'
root
...
$ cat /etc/passwd | perl -n -e 'if (/^(\w+):/) { print "$1\n" }'
root
...
```

`-p` 选项与 `-n` 相同，除了它在每行后打印 `$_`。

如果你组合 `-i` 选项，Perl 将就地编辑你的文件。因此，要将一堆文件从 DOS
转换成 UNIX 换行，你可以这样干：

```perl
$ perl -p -i -e 's/\r\n/\n/' file1 file2 file3
```

### perl -M

Perl 的 `-M` 选项使你可以从命令行使用模块。有好些模块首选此方式运行（如
*CPAN* 和 *Devel::Cover*）。如果你需要使用 `-e` 包含模块，它也是习惯的
简写。

```perl
$ perl -e 'use Data::Dumper; print Dumper( 1 );'
$VAR1 = 1;

$ perl -MData::Dumper -e 'print Dumper( 1 );'
$VAR1 = 1;
```

### 明白模块是否已被安装

试试从命令行加载模块。`-e1` 只是一个立即退出的空程序。如果你获得错误，
那么该模块未被安装：

```perl
$ perl -MWWW::Mechanize::JavaScript -e 1
Can't locate WWW/Mechanize/JavaScript.pm in @INC...
BEGIN failed--compilation aborted.
$
```

返回没有错误则意味着该模块已安装。

```perl
$ perl -MWWW::Mechanize -e 1
$
```

当它存在时，检查版本：

```perl
$ perl -MWWW::Mechanize -e'print $WWW::Mechanize::VERSION'
```

并非所有模块都有 `$VERSION` 变量，因此这可能不总是工作。
