---
layout: post
title: Programming Language Design ---- Environment
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---
#### Problem Description
The environment is a tree to store all the value we need for future use.
In the environment file it will have five basic functions

- create
- extend
- lookup
- insert
- update

#### functions
##### <span style="color:blue"> function define </span>
```
extern Lexeme *createEnv();
extern Lexeme *extendEnv(Lexeme *env, Lexeme *vars, Lexeme *vals);
extern Lexeme *makeTable(Lexeme *vars, Lexeme *vals);
extern Lexeme *lookupEnv(Lexeme *var, Lexeme *env);
extern int sameVariable(Lexeme *x, Lexeme *y);
extern Lexeme *insert(Lexeme *var, Lexeme *val, Lexeme *env);
extern Lexeme *updateEnv(Lexeme *var, Lexeme *env, Lexeme *newVariable);
```

##### <span style="color:blue">create environment </span>
```
Lexeme *createEnv()
{
    return extendEnv(NULL, NULL, NULL);
}
```

##### <span style="color:blue">extend environment </span>
```
Lexeme *extendEnv(Lexeme *env, Lexeme *vars, Lexeme *vals)
{
    return cons(ENVIRONMENT, makeTable(vars, vals), env);
}
```

##### <span style="color:blue">make table </span>
```
Lexeme *makeTable(Lexeme *vars, Lexeme *vals)
{
    return cons(TABLE, vars, vals);
}
```

##### <span style="color:blue">lookup environment </span>
```
Lexeme *lookupEnv(Lexeme *var, Lexeme *env)
{
    while (env != NULL)
    {
        Lexeme *table = car(env);
        Lexeme *vars = car(table);
        Lexeme *vals = cdr(table);
        while (vars != NULL)
        {
            if (sameVariable(var, car(vars)))
            {
                return car(vals);
            }
            //walk the lists in parallel
            vars = cdr(vars);
            vals = cdr(vals);
        }
        env = cdr(env);
    }
    fprintf(stderr, "FATAL: variable, %s, is undefined.", var->stringVal);
    return NULL;
}
```
##### <span style="color:blue">insert environment </span>
```
Lexeme *insert(Lexeme *var, Lexeme *val, Lexeme *env)
{
    Lexeme *table = car(env);
    setCar(table, cons(GLUE, var, car(table)));
    setCdr(table, cons(GLUE, val, cdr(table)));
    return val;
}
```
##### <span style="color:blue">update environment </span>
```
Lexeme *updateEnv(Lexeme *var, Lexeme *val, Lexeme *env)
{
     while (env != NULL)
    {
        Lexeme *table = car(env);
        Lexeme *vars = car(table);
        Lexeme *vals = cdr(table);
        while (vars != NULL)
        {
            if (sameVariable(var, car(vars)))
            {   
                setCar(var, val);
                return val;
            }
            //walk the lists in parallel
            vars = cdr(vars);
            vals = cdr(vals);
        }
        env = cdr(env);
    }
    fprintf(stderr, "FATAL: variable, %s, is undefined.", var->stringVal);
    return NULL;
}
```
