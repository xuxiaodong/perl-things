---
layout: post
title: 构造
date: 2013-10-28 11:07
post-link: http://perl101.org/constructs.html
---

### C 风格的循环通常不必要

你可以写 C 风格的循环，但常常不需要它们。

不要在 `foreach` 的位置使用它们：

```perl
for (my $i = 0; $i <= $#foo; $i++) { # BAD
    
foreach (@foo) { # BETTER
```

不要在 `while` 的位置使用它们：

```perl
for (my $i = <STDIN>; $i; $i = <STDIN>) { # BAD
        
while (my $i = <STDIN>) { # BETTER
```

想想你编写的代码，并找找感觉。

### 匿名哈希和数组

创建一个匿名数组引用，并给它赋值：

```perl
my $array = [ 'one', 'two', 'three' ];
```

匿名是因为我们不必创建数组。

哈希有相似的构造器：

```perl
my $hash = { one => 1, two => 2, three => 3 };
```

看作你应认为的而非引用。

### `q[qrwx]?//`、`m//`、`s///` 及 `y///`

Perl 让你自行指定定界符：

* 单引号：`'text' => q/text/`
* 双引号：`"text" => qq/text/`
* 正则表达式：`qr/text/`。除此之外，在 Perl
  匹配及替换操作符外没有别的方式指定正则表达式匹配。
* 单词：`("text", "text") => qw(text text);`
* 反引号：```text` => qx/text/``
* 正则匹配（`m//`）、正则替换（`s///`）、及转换(`tr///`、`y///`) 工作方式相同

你可以使用除空白之外的任意字符。但要注意平衡括号或花括号：

```perl
qq//
qq#A decent <html> delimiter </html> #
qq( man perl(1) for details ) # valid!
```

### `global`、`local`、`my` 及 `our`

* 使用 `use vars` 声明全局变量
* 使用 `my` 声明词法变量
* `local` 并非你所认为的，除非你知道为何使用 `local`，否则使用 `my` 代替
* 仅当你的包需要全局变量时使用 `our`
