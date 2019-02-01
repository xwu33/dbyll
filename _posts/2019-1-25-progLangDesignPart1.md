---
layout: post
title: Programming language design part I
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---

#### Programming Language Grammars design
You shuold design a gammar for your programming language.The example I am using contain 25 rules. The non-terminal variable is on the left the terminal variable is on the right. There is an example below.

unary : ID
<br>
			| NUMBER
<br>
			| OPEN_PAREN expression CLOSE_PAREN
<br>
			| STRING
<br>
			| ID OPEN_BRAKET CLOSE_BRAKET
<br>
			| INTEGER
---------------------------------------------------
[Project Link](https://github.com/scao7/cs403)

more information coming!
