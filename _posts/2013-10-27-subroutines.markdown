---
layout: post
title: '子例程'
date: 2013-10-27 10:14
post-link: http://perl101.org/subroutines.html
---

### 使用 `shift` 检索子例程的参数

子例程的参数来自于特殊的 `@_` 数组。不带参数的 `shift` 默认使用 `@_`。

```perl
sub volume {
    my $height = shift;
    my $width  = shift;
    my $depth  = shift;

    return $height * $width * $depth;
}
```

### 使用列表赋值来赋给子例程参数

你也可以使用列表赋值赋给子例程参数：

```perl
sub volume {
    my ($height, $width, $depth) = @_;

    return $height * $width * $depth;
}
```

### 通过访问 `@_` 直接处理子例程参数

在某些情况下，但我们希望很少，你能够通过 `@_` 数组直接访问参数。

```perl
sub volume {
    return $_[0] * $_[1] * $_[2];
}
```

### 传递的参数能被修改

传递给子例程的参数是实际参数的别名。

```perl
my $foo = 3;
print incr1($foo) . "\n"; # prints 4
print "$foo\n"; # prints 3

sub incr1 {
    return $_[0]+1;
}
```

如果你想要这种效果的话，这样更好：

```perl
sub incr2 {
    return ++$_[0];
}
```

### 子例程没有检查参数

如果你喜欢，你能够将任意东东传递给子例程。

```perl
sub square {
    my $number = shift;
            
    return $number * $number;
}

my $n = square( 'Dog food', 14.5, 'Blah blah blah' );
```

该函数只会使用第一个参数。因为这个关系，你可以使用任意数目的参数，
甚至没有参数来调用函数。

```perl
my $n = square();
```

Perl 不会对此抱怨。

*Params::Validate* 模块解决了许多验证问题。

### Perl 有原型，忽略它们

在演进的道路上加入了原型，因此你可以像这样干：

```perl
sub square($) {
    ...
}

my $n = square( 1, 2, 3 ); # run-time error
```

无论如何都不要使用它们。它们不会作用于对象，它们需要在调用子例程
前先予以声明。它们是好想法，但只是不实用。

### 利用 `BEGIN` 块在编译时做事

`BEGIN` 是一种特殊的代码块类型。它允许程序员在 Perl 的编译阶段执行
代码，这样可以执行初始化及做其他事情。

Perl 使用 `BEGIN` 在任意时导入模块。下列两个语句是等效的：

```perl
use WWW::Mechanize;

BEGIN {
    require WWW::Mechanize;
    import WWW::Mechanize;
}
```

### 传递数组及哈希引用

记住传给子例程的参数是作为一个大数组传递的。如果你像下面这样干：

```perl
my @stooges    = qw( Moe Larry Curly );
my @sandwiches = qw( tuna ham-n-cheese PBJ );

lunch( @stooges, @sandwiches );
```

那么传给 `lunch` 的是列表：

```perl
( "Moe", "Larry", "Curly", "tuna", "ham-n-cheese", "PBJ" );
```

在 `lunch` 中，你如何能告诉 stooges 结束及 sandwiches 开始的位置？
你不能。如果你尝试这样：

```perl
sub lunch {
    my (@stooges, @sandwiches) = @_;
```

那么所有 6 个元素都会跑到 `@stooges` 中，而 `@sandwiches` 什么都不会
得到。

答案是使用引用，正如：

```perl
lunch( \@stooges, \@sandwiches );

sub lunch {
    my $stoogeref   = shift;
    my $sandwichref = shift;

    my @stooges     = @{$stoogeref};
    my @sandwichref = @{$sandwichref};
    ...
}
```
