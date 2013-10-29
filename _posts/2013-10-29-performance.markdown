---
layout: post
title: 性能
date: 2013-10-29 09:26
post-link: http://perl101.org/performance.html
---

Perl 让你干想干的事，包括很慢或内存消耗这样的事。此处将告诉你如何避免。

### 使用 `while` 而非 `for` 来迭代整个文件

代替读取文件的所有行并使用 `for` 处理数组，使用 `while` 一次仅读取一行。

这两个循环的功能相同：

```perl
for ( <> ) {
    # do something
}

while ( <> ) {
    # do something
}
```

差异是 `for` 将整个文件读到临时数组，而 `while` 一次读一行。对于小文件，
这可能不重要。但对于更大的文件，它可能吃掉你的所有可用内存。

在这种情况下使用 `while` 是一种好实践。

### 避免不必要的引起和字串化

除非绝对必要，不要引起大字符串：

```perl
my $copy = "$large_string";
```

这将创建两个 `$large_string` 的拷贝（一个是 `$copy`，而另一个是引起）。
然而：

```perl
my $copy = $large_string;
```

仅创建一个拷贝。

对于字串化大的数组也是一样：

```perl
{
    local $, = "\n";
    print @big_array;
}
```

这比下面的代码更内存高效：

```perl
print join "\n", @big_array;
```

或：

```perl
{
    local $" = "\n";
    print "@big_array";
}
```
