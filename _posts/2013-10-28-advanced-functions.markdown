---
layout: post
title: 高级函数
date: 2013-10-28 16:12
post-link: http://perl101.org/advanced-functions.html
---

### 上下文与 wantarray

Perl 有三种上下文：空、标量、以及列表。

```perl
            func(); # void
my $ret   = func(); # scalar
my ($ret) = func(); # list
my @ret   = func(); # list
```

如果你在子例程或 `eval` 块中，你能够使用 `wantarray` 来决定想要的上下文。

以下是处理正则表达式返回值的上下文例子：

```perl
my $str = 'Perl 101 Perl Context Demo';
my @ret = $str =~ /Perl/g; # @ret = ('Perl','Perl');
my $ret = $str =~ /Perl/g; # $ret is true
```

### .. 和 ...

这些叫区间操作符，它们能够帮助代码处理整数或字符区间。

在下例中，`@array` 是手动填充的。这些是等价的：

```perl
my @array = ( 0, 1, 2, 3, 4, 5 );
my @array = 0..5;
```

当用于此种方式时，`..` 和 `...` 是等效的。

区间操作符只能增加。这会产生一个空列表：

```perl
my @array = 10..1; # @array is empty
```

如果你想要逆向，要求它。

```perl
my @array = reverse 1..10; # @array descends from 10 to 1
```

你也可以在标量上下文中使用区间操作符，但那超出了本节的范围。参阅手册页
了解细节。
