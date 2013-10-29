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
