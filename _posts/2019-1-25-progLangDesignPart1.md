---
layout: post
title: Programming language design part I
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---

#### Programming Language Grammars design
1. You shuold design a gammar for your programming language.The example I am using contain 25 rules. The non-terminal variable is on the left the terminal variable is on the right. There is an example below.

`BNF style`

```bash
unary : ID
			| NUMBER
			| OPEN_PAREN expression CLOSE_PAREN
			| STRING
			| ID OPEN_BRAKET CLOSE_BRAKET
			| INTEGER
```
[========]
[Project Link](https://github.com/scao7/cs403)

more information coming!
