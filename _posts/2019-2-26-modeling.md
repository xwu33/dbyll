---
layout: post
title: webGL 3D Modeling and Transformation
categories: [Programming Language]
tags: [Programming Language]
fullview: false
comments: false
---

#### Problem Description
In this project, we need to use webGL to model a 3D screen with display a paragrapgh you will be able to rotate the screen either by using the button on the screen or the arrow key on the keyboard.

#### Approach
using 16 segment display method to display the character.

##### <span style="color:blue">left segment example </span>
```JavaScript
function left1() {
  var s = scale4(0.25, 1.5, 3);
  var instanceMatrix = mult(translate(-1.5, 0.75, 0.0), s);
  var t = mult(modelViewMatrix, instanceMatrix);
  gl.uniformMatrix4fv(modelViewMatrixLoc, false, flatten(t));
  gl.drawArrays(gl.TRIANGLES, 0, NumVertices);
}
```
#### construct letter
calling the segment function to form a letter
##### <span style="color:blue">letter 'A' example </span>
```JavaScript
function a() {
  left1();
  left2();
  right1();
  right2();
  TLeft();
  TRight();
  midLeft();
  midRight();
}
```
#### construct words
Using for loop and model View Transformation to display the words.

##### <span style="color:blue">for loop display words </span>
```JavaScript
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
```
#### parser paragraph
parsering the input paragraph and make delay
##### <span style="color:blue">parser and delay example </span>
```JavaScript
function isLetter(str){
  return str.match(/[a-z]/i)||str.match(/[A-Z]/i);
}

var index =0;
  var word = "";
var intervalID = window.setInterval(function(){
  console.log(name);
  console.log(name.length);
  word = "";
  if (index >= name.length)
    index = 0;
  while(name[index] != " "&& index != name.length){
    if(isLetter(name[index]))
    word += name[index];
    index ++;
  }
  index ++;
  word = word.toLowerCase();
  console.log(word);

}, 1000);
```
click the project link to see the demo
[Project Link](https://scao7.github.io/cs435/project3/modeling.html)
