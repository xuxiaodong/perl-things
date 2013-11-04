---
layout: post
title: '流程控制'
date: 2013-10-26 16:55
post-link: http://perl101.org/flow-control.html
---

### 四个假值

在 Perl 中有 4 种假值：

```perl
my  $false = undef;
    $false = "";
    $false = 0;
    $false = "0";
```

最后一个为假值是因为 `"0"` 在数字上下文中将变成 0，根据第三条规则，
它是假值。

### 后缀控制

简单的 `if` 或 `unless` 块可能看起来像这样：

```perl
if ($is_frobnitz) {
    print "FROBNITZ DETECTED!\n";
}
```

在这些情况下，`if` 或 `unless` 能够追加到简单语句的尾部。

```perl
print "FROBNITZ DETECTED!\n" if $is_frobnitz;
die "BAILING ON FROBNITZ!\n" unless $deal_with_frobnitz;
```

`while` 和 `for` 也可以这样用。

```perl
print $i++ . "\n" while $i < 10;
```

### `for` 循环

`for` 循环有三种风格。

```perl
my @array;

# Old style C for loops
for (my $i = 0; $i < 10; $i++) {
    $array[$i] = $i;
}

# Iterating loops
for my $i (@array) {
    print "$i\n";
}

# Postfix for loops
print "$_\n" for @array;
```

你也许会看到 `foreach` 用于 `for` 的位置。它们两个可以互换。在上述后
两种循环风格中多数人使用 `foreach`。

### `do` 块

`do` 允许 Perl 在期待语句的位置使用块。

```perl
open( my $file, '<', $filename ) or die "Can't open $filename: $!"
```

但如果你需要做别的事：

```perl
open( my $file, '<', $filename ) or do {
    close_open_data_source();
    die "Aborting: Can't open $filename: $!\n";
};
```

下列代码也是等价的：

```perl
if ($condition) { action(); }
do { action(); } if $condition;
```

作为特殊情况，`while` 至少执行块一次。

```perl
do { action(); } while action_needed;
```

### Perl 没有 `switch` 或 `case`

如果你从其他语言而来，你可能用过 `case` 语句。Perl 没有它们。

最接近的我们有 `elsif`：

```perl
if ($condition_one) { 
    action_one();
}
elsif ($condition_two) {
    action_two();
}
...
else {
    action_n();
}
```

没有办法像 `case` 那样清晰。

### `given ... when`

从 Perl 5.10.1 开始，你可以使用下列代码来打开实验性的 `switch` 特性：

```perl
use feature "switch";

given ($var) {
    when (/^abc/) { $abc = 1 }
    when (/^def/) { $def = 1 }
    when (/^xyz/) { $xyz = 1 }
    default       { $nothing = 1 }
}   
```

### `next/last/continue/redo`

考虑以下循环：

```perl
$i = 0;
while ( 1 ) {
    last if $i > 3;
    $i++;
    next if $i == 1;
    redo if $i == 2;
}
continue {
    print "$i\n";
}
```

输出：

```perl
1
3
4
```

`next` 跳到块尾并继续或重新开始。

`redo` 立即跳回到循环的开头。

`last` 跳到块尾并阻止循环再次执行。

`continue` 在块尾执行。
