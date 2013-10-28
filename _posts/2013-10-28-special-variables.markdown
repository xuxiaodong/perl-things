---
layout: post
title: 特殊变量
date: 2013-10-28 14:05
post-link: http://perl101.org/special-variables.html
---

### $\_

`$_` 是默认变量。它常用于内置函数的默认参数。

```perl
while ( <> ) {  # Read a line into $_
    print lc;   # print lc($_)
}
```

这与下列代码相同：

```perl
while ( $it = <> ) {
    print lc($it);
}
```

### $0

`$0` 包含执行程序的名称，正如给 Shell 的一样。如果程序直接通过 Perl
解释器执行，那么 `$0` 包含文件名称。

```perl
$ cat file.pl
#!/usr/bin/perl
print $0, "\n";
$ ./file.pl
file.pl
$ perl file.pl
file.pl
$ perl ./file.pl
./file.pl
$ cat file.pl | perl
-
```

`$0` 是 C 程序员期望从 `argv` 数组找到的第一个元素。

### @ARGV

`@ARGV` 包含给程序的参数，顺序与 Shell 中一样。

```perl
$ perl -e 'print join( ", ", @ARGV), "\n"' 1 2 3
1, 2, 3
$ perl -e 'print join( ", ", @ARGV), "\n"' 1 "2 3" 4
1, 2 3, 4
```

C 程序员可能会搞混，因为 `$ARGV[0]` 是他们的 `argv[1]`。不要犯这样的错。

### @INC

`@INC` 包含 Perl 搜索模块的所有路径。

Perl 程序员通过后置或前置到 `@INC` 添加库路径。眼下，使用 `use lib` 代替。
下面的代码等效：

```perl
BEGIN { unshift @INC, "local/lib" };

use lib "local/lib";
```

### %ENV

`%ENV` 包含当前环境的拷贝。该环境由 Perl 创建的子 Shell 所给予。

这对 `taint` 模式很重要，`%ENV` 具有能修改 Shell 行为的内容。正因如此，
perlsec 推荐在 `taint` 模式执行命令时使用下列代码：

```perl
$ENV{'PATH'} = '/bin:/usr/bin'; # change to your real path
delete @ENV{'IFS', 'CDPATH', 'ENV', 'BASH_ENV'};
```

### %SIG

Perl 具有丰富的信号处理能力。使用 `%SIG` 变量，你能够在当信号发送给运行
的进程时执行任意子例程。

如果你有耗时进程，这将特别有用。通过发送信号（通常是 `SIGHUP`）来重载配置
，你不必启动和停止进程。

通过分别赋值 `$SIG{__DIE__}` 和 `$SIG{__WARN__}`，你也可以更改 `die` 和 `warn`
的行为。

### <>

钻石操作符 `<>` 用于程序期望的输入时，而不用关心它如何到达。

如果程序收到任何参数，它们将分成文件名及其内容发送给 `<>`。否则，使用标准
输入（`STDIN`）。

`<>` 对于过滤程序特别有用。

### \<DATA\> 和\_\_DATA\_\_

如果程序包含自身为一行的魔法标记 `__DATA__`，那么它下面的任何东东均可通过
魔法 `<DATA>` 句柄为程序所用。

如果你想在程序中包含数据，但又想与主程序逻辑分开，那么这将特别有用。

### $!

当使用 `system` 执行命令时，如果命令返回非真状态，那么 `$!` 将为真。否则，
可能未被执行。`$!` 将包含出错消息。

### $@

如果使用 `eval`，那么 `$@` 将包含 `eval` 所抛出的语法错误。
