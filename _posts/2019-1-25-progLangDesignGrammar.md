---
layout: post
title: Programming language design ---- Grammars
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---
#### Problem Description
The task is to build an interpreter for a general purpose programming language of my own design. The language must - support the following features:

- comments
- integers and strings
- dynamically typed (like Scheme and Python)
- classes/objects
- arrays with O(1) access time
- conditionals
- recursion
- iteration
- convenient means to access command line arguments
- convenient means to print to the console
- convenient means to read integers from a file
- an adequate set of operators
- anonymous functions
- functions as first-class objects (i.e. functions can be manipulated as in Scheme - e.g. local functions)
- (graduate only) an inheritance system and detection of variables used before definition

The only basic types you need to provide are integer and string and you do not need to provide methods for coercing one type to another (although you may find it convenient to do so). The efficiency of your interpreter is not particularly important, as long as you can solve the test problem in a reasonable amount of time. Your language also does not need to support reclamation of memory that is no longer needed. You are to write your program in a statically-typed, imperative language such as C, C++, or Java. Check with me first if you wish to use some other host language.

#### BNF programming language design style
Example:
```
unary : NUMBER
      | VARIABLE
```
Left is the general rule, right is the specific. The syle describe how the no-terminal is broken down to a small pieces.
#### BNF grammar design for my language

##### The Name of the operator and symbol define:

>QUATE              """
ASSIGN             "="
OPREN              "("
CPPREN             ")"
OBRACE             "{"
CBRACE             "}"
OBRAKET            "[ "  
CBRAKET            "]"
SEMICOLON          ";"
COLON              ":"
COMMA              ","
EQUALS             "=="
NOTEQUALS          "!="
LESSTAN            "<"
GREATERTHAN        ">"
LESSOREQUAL        "<="
GREATEROREQUAL     ">="
PLUS               "+"
TIMES              "*"
DIVIDE             "/"
SUBTRACT           "-"
MODULUS            "%"
AND                "&&"
OR                 "||"

##### unary
> test

[Project Link](https://github.com/scao7/cs403)

more information coming!
