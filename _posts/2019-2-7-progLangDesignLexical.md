---
layout: post
title: Programming language design ---- Lexical Analysis
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---
#### problem description
When implementing a programming language, the first step is reading in the source code of a program written in that language. Typically, the source code is stored as a file of characters. To read in a source code file, one groups the important individual characters into tokens and discards the unimportant characters. For example, consider the Python program:

    print 'Hello World!'

There are two tokens in this program, print and 'Hello World!'. The unimportant characters are the space that follows the token print and the newline that follows the token 'Hello World'. Note that the space within the token 'Hello World!' is important, so the subsystem for reading in source code must be smart enough to distinguish between important and unimportant spaces, among other things. This subsystem is called lexical analysis.

- types
- lexeme
- lexer
- scanner

#### lexical analysis approach
The is a program that read the file you have, it will out put a sequence of lexeme until the end of the line.

This is the .h file to define different token names and corresponding with grammar.
I put partial example here:
##### <span style="color:blue">types.h</span>
``````
#define OPREN "OPREN"
#define FUNCTION_DEF "func"
``````
 The approach here is to have a lexeme.c data structure to help me solve the problem.
##### <span style="color:blue">lexeme.c</span>
``````
typdef structure lexeme {
  char* type;
  char* string;
  int integer;
  double real;
  struct lexeme* left; // reserved for parsing
  struct lexeme* right;// reserved for parsing
}
``````
Another part is the lexer to determine each token
##### <span style="color:blue">lexer.h</span>
``````
lexeme* lex(void);
void newLexer(char* file);
void skipWhiteSpace(); // for comments
lexeme* lexNumber();
lexeme* lexVariable();
lexeme* lexString();
lexeme* lexUnknown();
``````
##### <span style="color:blue">lexer.h</span>

[Project Link](https://github.com/scao7/cs403)

to be continue! last revise 2/72019
