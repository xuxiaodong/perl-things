---
layout: post
title: 外部程序
date: 2013-10-28 10:30
post-link: http://perl101.org/external-programs.html
---

在 Perl 中有三种方式来调用外部程序。

### `system()` 返回程序的退出状态

```perl
my $rc = system("/bin/cp $file1 $file2"); # returns exit status values
die "system() failed with status $rc" unless $rc == 0;
```

如果可能，用列表传递你的参数，而不是用单个的字符串。

```perl
my $rc = system("/bin/cp", $file1, $file2 );
```

如果 `$file1` 或 `$file2` 有空格或其他特殊字符，这将确保不会在 Shell
中出错。

`system()` 的输出不会被捕获。

### 反引号（\`\`）和 `qx()` 操作符返回程序的输出

当你想要输出时，使用：

```perl
my $output = `myprogram foo bar`;
```

你将需要检查 `$!` 中的错误代码。

如果你使用反引号或 `qx()`，首选 *IPC::Open2* 或 *IPC::Open3* 代替，因为
它们将给你相同的参数控制，并允许你捕获输出。

`IPC::Open3` 是在 Perl 中不使用 Shell 命令来捕获 `STDERR` 的仅有方法。
