---
layout: post
title: 调试
date: 2013-10-28 09:01
post-link: http://perl101.org/debugging.html
---

### 开启 `strict` 和 `warnings`

无论何时调试代码，都确信已开启了 `strict` 和 `warnings` 编译指令。

将下面两行：

```perl
use strict;
use warnings;
```

放到你试图调试或将来可能想调试的程序的顶部。

[strict][s] 编译指令强制你使用那些允许 Perl 在编译时找出错误的许多特性。
首先最重要的是，在 `strict` 下，变量必须在使用前声明。多数情况下，这意味
着使用 `my`：

```perl
use strict;
my $foo = 7;                # OK, normal variable
print "foo is $fooo\n";     # Perl complains and aborts compilation
```

没有 `strict`，Perl 仍然会高兴地执行上面的程序。但你可能会感到杯具，想不
明白 `$foo` 为何没有值。启用 `strict` 也会减少许多令人头痛的输入错误。

另外，`strict` 不允许使用多数裸字。

```perl
no strict;
$foo = Lorem;
print "$foo\n";         # Prints "Lorem"

use strict;
my $foo = ipsum;        # Complains about bareword
$foo = (
    Lorem   => 'ipsum'  # OK, barewords allowed on left of =>
);

$SIG{PIPE} = handler;   # Complains
$SIG{PIPE} = \&handler; # OK
$SIG{PIPE} = "handler"; # Also, OK, but above is preferred
```

最后，如果你使用[符号引用][r]，启用 `strict` 将抛出运行时错误。

```perl
no strict;
$name = "foo";
$$name = "bar";             # Sets the variable $foo to 1
print "$name $$name\n";     # Prints "foo bar"

use strict;
my $name = "foo";
$$name = "bar";             # Complains: can't use "foo" as ref
```

`warnings` 编译指令将使 Perl 吐出许多有用的抱怨，以便让你知道程序中的
某个东东并非你所想要的：

```perl
use warnings;
my $foo = ;
$foo += 3;
my $foo = 1;            # Compains: redeclaration of variable

my $bar = '12fred34';
my $baz = $bar + 1;     # Complains: Argument "12fred34" isn't numeric
                        # Complains: Name "main::baz" used only once
```

参阅 strict 及 warnings 的文档了解其他信息。关于不用 `strict` 所带来的
恐怖故事，可以看看 [PerlMonks][m] 上面的帖子。

### 检查每个 `open` 的返回值

你将经常看到人们抱怨下面的代码无法执行：

```perl
open( my $file, $filename );
while ( <$file> ) {
    ...
}
```

接着抱怨 `while` 循环也被破坏了。这儿的常见问题是文件 `$filename` 实际
上并不存在，因此 `open` 将失败。如果没有检查，那么你将从来不会知道。使
用以下代码替换它：

```perl
open( my $file, '<', $filename ) or die "Can't open $filename: $!";
```

### 利用 `diagnostics` 扩展警告

有时候警告消息并没有解释你想要的那么多。例如，为何你会获得此警告？

```
Use of uninitialized value in string eq at /Library/Perl/5.8.6/WWW/Mechanize.pm line 695.
```

试试将下列内容放到程序顶部并重新执行代码：

```perl
use diagnostics;
```

现在警告看起来像这样：

```
Use of uninitialized value in string eq at /Library/Perl/5.8.6/WWW/Mechanize.pm
    line 695 (#1)
(W uninitialized) An undefined value was used as if it were already
defined.  It was interpreted as a "" or a 0, but maybe it was a mistake.
To suppress this warning assign a defined value to your variables.

To help you figure out what was undefined, perl tells you what operation
you used the undefined value in.  Note, however, that perl optimizes your
program and the operation displayed in the warning may not necessarily
appear literally in your program.  For example, "that $foo" is
usually optimized into "that " . $foo, and the warning will refer to
the concatenation (.) operator, even though there is no . in your
program.
```

更多的信息将帮助你找出程序的问题。

记住你也可以从命令行指定模块和编译指令，因此你甚至不必编辑源代码来使用
`diagnostics`。使用 `-M` 命令行选项再次执行它：

```perl
perl -Mdiagnostics mycode.pl
```

### 使用优化信号来获得栈跟踪信息

有时候你将获得警告，但你并不明白是如何得到的。比如：

```
Use of uninitialized value in string eq at /Library/Perl/5.8.6/WWW/Mechanize.pm line 695.
```

你可以看到模块的 695 行代码干了什么，但你无法看到你的代码在此时干了什么。
你将需要看到子例程被调用的跟踪信息。

当 Perl 调用 `die` 或 `warn` 时，它将分别指定 `$SIG{__DIE__}` 和
`$SIG{__WARN__}` 来通过子例程。如果你修改它们，让其成为比 `CORE::die` 和
`CORE::warn` 更有用的话，你就得到了一个有用的调试工具。这种情况，可以使用
`Carp` 模块的 `confess` 函数。

在你程序的顶部，添加这些行：

```perl
use Carp qw( confess );
$SIG{__DIE__} =  \&confess;
$SIG{__WARN__} = \&confess;
```

现在，当代码调用 `warn` 或 `die` 时，`Carp::confess` 函数将处理它。`confess`
打印原始警告，跟着栈跟踪信息，然后停止执行程序。

```perl
Use of uninitialized value in string eq at /Library/Perl/5.8.6/WWW/Mechanize.pm line 695.
    at /Library/Perl/5.8.6/WWW/Mechanize.pm line 695
        WWW::Mechanize::find_link('WWW::Mechanize=HASH(0x180e5bc)', 'n', 'undef') called at foo.pl line 17
        main::go_find_link('http://www.cnn.com') called at foo.pl line 8
```

现在我们有更多信息来调试代码，包括精确的调用函数及传递的参数。从这儿，
我们能够容易地看到 `find_link` 的第三个参数是 `undef`，那将是一个开始
调查的好地方。

### 使用 Carp::Always 获得栈跟踪信息

如果你不想覆盖信号处理器，那么可以安装 CPAN 模块 `Carp::Always`。

在安装之后，添加下行到你的代码中：

```perl
use Carp::Always;
```

或者使用 `-MCarp::Always` 从命令行调用你的程序，这将总是会得到栈跟踪信息。

[s]: http://perldoc.perl.org/strict.html
[r]: http://perldoc.perl.org/perlref.html#Symbolic-references
[m]: http://www.perlmonks.org/?node_id=482733
