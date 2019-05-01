---
layout: post
title: Programming language design ---- Grammars
categories: [Programming Language]
tags: [Programming Language,C]
fullview: false
comments: false
shortinfo: The task is to build an interpreter for a general purpose programming language of my own design. The language must - support the following features:

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
The lower case name is the non-terminal words, capitalized case is the terminal words.
#### BNF grammar design for my language

##### The name of the operator and symbol define:
```
QUATE              """
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
CARET              "^"
POUND              "#"
NOT                "!"
```
##### <span style="color:blue">reserved keywords</span>
```
keywords : show
         | if
         | goto
         | else
         | until
         | call
         | var
         | func
         | loop
         | when
```
##### <span style="color:blue">unary</span>
```
unary : VARIABLE
      | NUMBER
      | OPREN expression CPREN
      | STRING
      | VARIABLE OBRAKET optExpression CBRAKET
      | NULL
```
##### <span style="color:blue">operators</span>
```
operator : PLUS
         | SUBTRACT
         | TIMES
         | DIVIDE
         | CARET
         | ASSIGN
         | condition
```
##### <span style="color:blue">condition</span>
```
condition : LESSTAN
          | GREATERTHAN
          | GREATEROREQUA
          | LESSOREQUAL
          | NOT
          | EQUALS
          | OR
          | AND
```
##### <span style="color:blue">expression</span>
```
expression : unary
           | unary oprator expression
           | printExp
           | functionDefExp
           | functionCallExp
           | variableDefExp
```
##### <span style="color:blue">print expression</span>
```
printExp : show unary 					 
```
##### <span style="color:blue">expression list</span>
```
expressionList : expression
               | expression COMMA expressionList
```
##### <span style="color:blue">optional expression</span>
```
optExpression : expressionList
              | *empty*
```
##### <span style="color:blue">statement</span>
```
statement : expression SEMICOLON
          | loopStatment
          | ifStatment
```
##### <span style="color:blue">statements</span>
```
statements : statement
           | statement statements
```
##### <span style="color:blue">parameter list</span>
```
parameterList : unary
              | unary COMMA parameterList
```
##### <span style="color:blue">optional parameter list</span>
```
optParameterList : parameterList
                 | *empty*
```
##### <span style="color:blue">function define expression</span>
```
functionDefExp : func VARIABLE OPREN optExpression CPREN block
```
##### <span style="color:blue">function call expression</span>
```
functionCallExp : call VARIABLE
                | call VARIABLE OPREN optExpression CPREN
```
##### <span style="color:blue">variable define expression</span>
```
variableDefExp : var VARIABLE
               | var VARIABLE ASSIGN unary
```
##### <span style="color:blue">if statement</span>
```
ifStatment : if OPREN expression CPREN block
           | elseStatement
```
##### <span style="color:blue">else statement</span>
```
elseStatement : else block
              | else ifStatment
              | *empty*
```
##### <span style="color:blue">return statement</span>
```
returnStatement : return SEMICOLON
                | return unary SEMICOLON
```
##### <span style="color:blue">program</span>
```
program : statements
```
##### <span style="color:blue">block</span>
```
block : OBRACE statements CBRACE
      | OBRACE *empty* CBRACE
```
[Project Link](https://github.com/scao7/cs403)

to be continue! last revise 2/72019
