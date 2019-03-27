---
layout: post
title: Programming language design ---- Recognizer
categories: [Programming Language]
tags: [Programming Language,C]
fullview: false
comments: false
---

#### Problem Description
A recognizer is like a souped-up scanner; Like a scanner, it repeatedly calls lex to serve up lexemes. Additionally, it checks each lexeme against a grammar that describes the language in the text being scanned. Ultimately, a recognizer reports whether the entire text is syntactically correct or not.

#### Recognizer Approach
- Recursive descent parsing
- Transforming grammars
- Support functions for recursive descent parsing
- Recognizing expressions
- Conditionals and iterations

#### Read file and construct

##### <span style="color:blue">parse</span>
``````
Lexeme *parse(FILE *inputFile)
{
    Parser *p = malloc(sizeof(Parser));
    p->fIn = inputFile;
    p->line = 1;
    p->pending = lex(p);
    p->tree = program(p);
    return p->tree;
}
``````

#### parser rule

##### <span style="color:blue">primary</span>
```
Lexeme *primary(Parser *p)
{
    Lexeme *x, *y = NULL;
    if (literalPending(p))
    {
        return literal(p);
    }
    else if (check(p, BREAK))
    {
        return match(p, BREAK);
    }
    else if (check(p, OPREN))
    {
        match(p, OPREN);
        x = expr(p);
        match(p, CPREN);
        return x;
    }
    else if (lambdaPending(p))
    {
        x = lambda(p);
        if (check(p, OPREN))
        {
            match(p, OPREN);
            y = optParamList(p);
            match(p, CPREN);
            return cons(FUNCCALL, x, y);
        }
        return x;
    }
    else if (check(p, NIL))
    {
        return match(p, NIL);
    }
    else if (check(p, IDENTIFIER))
    {
        x = match(p, IDENTIFIER);
        if (check(p, OBRACKET))
        {
            match(p, OBRACKET);
            y = expr(p);
            match(p, CBRACKET);
            return cons(ARRAYACCESS, x, y);
        }
        else if (check(p, OPREN))
        {
            match(p, OPREN);
            y = optParamList(p);
            match(p, CPREN);
            return cons(FUNCCALL, x, y);
        }
        else if (check(p, DOT))
        {
            y = match(p, DOT);
            y->left = x;
            y->right = primary(p);
            return y;
        }
        return x;
    }
    else
    {

        Fatal("Primary not found.");
        exit(1);
    }
    return NULL;
}
```


To be continue! last revise 2/26/2019
