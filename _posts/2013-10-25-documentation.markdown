---
layout: post
title: '文档'
date: 2013-10-25 11:18
post-link: http://perl101.org/documentation.html
---

### perldoc

`perldoc` 是 Perl 完整的语言参考工具。

* `perldoc perldoc`：用法介绍
* `perldoc perl`：文档概述
* `perldoc Module`：模块文档

### 利用 `perldoc -f` 查阅 Perl 函数

    $ perldoc -f system

    system LIST
    system PROGRAM LIST
            Does exactly the same thing as "exec LIST", except that a fork
            is done first and the parent process waits for the child
    ...

### 利用 `perldoc -v` 查阅 Perl 变量

    $ perldoc -v '$/'

    IO::Handle->input_record_separator( EXPR )
    $INPUT_RECORD_SEPARATOR
    $RS
    $/      The input record separator, newline by default.  This
    ...

### 利用 `perldoc -q` 搜索 Perl FAQ

    $ perldoc -q database

    Found in /usr/share/perl/5.18/pod/perlfaq8.pod
    How do I use an SQL database?

        The DBI module provides an abstract interface to most database servers
        and types, including Oracle, DB2, Sybase, mysql, Postgresql, ODBC, and
    ...

### 利用 `perldoc modulename` 查阅 Perl 模块文档

如果模块已经安装到你的系统中，那么你可以通过 `perldoc` 阅读该模块的文档。

    $ perldoc WWW::Mechanize

    NAME
        WWW::Mechanize - Handy web browsing in a Perl object

    VERSION
        Version 1.73

    SYNOPSIS
        "WWW::Mechanize", or Mech for short, is a Perl module for stateful
        programmatic web browsing, used for automating interaction with
        websites.
    ...

对于没有安装的模块，你将需要使用 <http://search.cpan.org> 或 `cpandoc`。

### 利用 `cpandoc` 查阅未安装 Perl 模块的文档

*Pod::Cpandoc* 模块提供了 `cpandoc` 工具，利用该工具，即便模块没有安装到
系统上，但你仍然能够查阅该模块的文档。

    $ cpandoc Web::Scraper

    NAME
        Web::Scraper - Web Scraping Toolkit using HTML and CSS Selectors or
        XPath expressions

    SYNOPSIS
        use URI;
        use Web::Scraper;
    ...

### 在线文档

一些网站维护有 Perl 的 HTML 文档，最大的两个站点是：

1. <http://perldoc.perl.org>：语言、函数及标准库
2. <http://search.cpan.org>：模块

### 编写自己的文档

Perl 具有浓烈的文档文化，我们鼓励你早日养成此习惯。你将使用一种叫做 POD
的格式来编写文档。
