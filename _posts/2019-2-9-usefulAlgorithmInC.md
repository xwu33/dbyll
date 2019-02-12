---
layout: post
title: Some useful data structure and algorithms in C
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---
#### Problem Description
Form some coding problem in the interview, I notice that it is harder for a person to use C to finish one question. The standard library in C is much less than C++, Java, Python and any other language. I want to document several method and function that included in C++ library but not in C library.

#### String Operation Functions C++ 
##### <span style="color:blue">String </span>
[redirect to cplusplus]: http://http://www.cplusplus.com/reference/string/string/
``````
#include <string>
string str; //declare
str.begin(); //first char
str.end(); //last char
str.rbegin(); //reverse begining 
str.rend();//reverse end
/*****capacity******/
str.size();
str.length();
str.capacity(); // the total space of string
str.max_size(); //maximun size the string can be
str.resize(size +2, '+');//resize and add two + to it  
str.resize(14);
str.empty(); // bool value to check if empty()
/****Element access*****/
str[];
str.at(i); // get the char at i
/****modifier******/
str += "string";
str += 'c';
str.append(string);
str.append(str,5,5); // append from location 5 and lenth after 5
str.append(10u,'.'); //..........
str.push_back();
str.assign("string");
str.compare(str1);// == 0 match != 0 not match
str.find("live");//pass in string return location


``````
#### String Operation Functions C
``````
``````
#### Sorting Algorithms
#### <span style="color:blue">Bubble </span>
``````

``````
#### Divide Conquer Problems

``````

``````



To be continue! last revise 2/9/2019
