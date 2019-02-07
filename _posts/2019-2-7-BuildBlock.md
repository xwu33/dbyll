---
layout: post
title: webGL basic block operation
categories: [algorithms]
tags: [grapics]
fullview: false
comments: false
---
#### problem description
Use WebGL to implement an online program called BuildingBlocks that allows a user to build something meaningful from the following six building blocks.
1.	Red disk
2.	Green disk
3.	Blue disk
4.	Magenta square
5.	Cyan square
6.	Yellow square

To add a block by holding a number key on the keyboard and left click.
To move a block by clicking on a block and drag.
To delete a block by holding a shift key and click on the block.

#### key problem
##### 1. create blocks
The first step about creating blocks should consider the further functionality
such as move and delete. So my solution here is to create a function called CPiece
to create the block.
```javascript
function CPiece (n, color, x0, y0, x1, y1, x2, y2, x3, y3) {
  if(cIndex < 3){ //draw circle
    this.NumVertices = 100 ;
    this.color = color;
    this.points=[];
    var radius =20;
    for(var i=0; i< this.NumVertices; i++ ){
      var x = radius * Math.cos((i / this.NumVertices) * 2.0 * Math.PI)+x0;
      var y = radius * Math.sin((i / this.NumVertices) * 2.0 * Math.PI)+y0;
      this.points.push(vec2(x, y));
    }
    console.log(this.NumVertices);
    this.colors=[];
    for(var i=0; i< this.NumVertices; i++ ){
      this.colors.push(color);
    }
  }
  else{//draw square
    this.NumVertices = n + 1 ;
    this.color = color;
    this.points=[];
    this.points.push(vec2(x0, y0));
    this.points.push(vec2(x1, y1));
    this.points.push(vec2(x2, y2));
    this.points.push(vec2(x3, y3));
    this.points.push(vec2(x1, y1));
    this.colors=[];
    for (var i=0; i<5; i++) this.colors.push(color);
  }
```
##### 2. select blocks
##### 3. re-render the canvas
##### 4. delete blocks

[Project Link](https://scao7.github.io/cs435/project2/Buildingblocks.html)
