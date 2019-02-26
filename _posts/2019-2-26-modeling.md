---
layout: post
title: Programming language design ---- Recognizer
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---

#### Problem Description
In this project, we need to use webGL to model a 3D screen with display a paragrapgh you will be able to rotate the screen either by using the button on the screen or the arrow key on the keyboard.

#### Approach
using 16 segment display method to display the character. Using for loop and model View Transformation to display the words.
```
modelViewMatrix = mult(modelViewMatrix,rotate(theta[0], 1,0,0));
modelViewMatrix = mult(modelViewMatrix,rotate(theta[1], 0,1,0));

modelViewMatrix = mult(modelViewMatrix, scalem(0.3, 0.3, 0.3));
modelViewMatrix = mult(modelViewMatrix, translate(-22, 10, 0.0));

mdvStatic = rotate(15, 1, 1, 0);
mdvStatic = mult(mdvStatic, scalem(0.3, 0.3, 0.3));
mdvStatic = mult(mdvStatic, translate(25, 10, 0.0));

for (var lo = 0; lo < 12; lo++) {
  printLetter(lo);
  modelViewMatrix = mult(modelViewMatrix, translate(4, 0.0, 0.0));
}
}
```
[Project Link](https://scao7.github.io/cs435/project3/modeling.html)
