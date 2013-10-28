---
layout: post
title: 引用
date: 2013-10-28 13:24
post-link: http://perl101.org/references.html
---

### 引用是引用其他变量的标量

引用像 C 中引用其他变量的指针。使用 `\` 操作符创建引用。

```perl
my $sref = \$scalar;
my $aref = \@array;
my $href = \%hash;
my $cref = \&subroutine;
```

引用指向的事物即其所指。

使用合适的印记解引用，首选使用花括号。

```perl
my $other_scalar = ${$sref};
my @other_array  = @{$aref};
my %other_hash   = %{$href};
&{$cref} # Call the referent.
```

### 用箭头符解引用更容易

要访问数组和哈希引用，使用 `->` 操作符。

```perl
my $stooge = $aref->[1];
my $stooge = $href->{Curly};
```

### ref vs. isa

* 一个引用属于一个类
* 你可以使用 `ref` 查检类
* 一个对象引用能从其他类继承
* 你可以使用 `isa` 来询问一个对象是否继承自一个类
* 没有好理由不要用 `ref`
* `isa` 是 *UNIVERSAL* 包的一部分，因此你可以在对象上调用它

```perl
my $mech = WWW::Mechanize->new;
print "ok\n" if $mech->isa('LWP::UserAgent');
```

### 引用匿名子例程

子例程能被赋给变量，并被调用，以允许代码引用被传递及使用。这将十分有用，
比如编写需要执行所提供代码的子例程。

```perl
my $casefix = sub { return ucfirst lc $_[0] };

my $color = $casefix->("rED");
print "Color: $color\n"; # prints Red
```
