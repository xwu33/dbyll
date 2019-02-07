---
layout: post
title: webGL basic block operation
categories: [algorithms]
tags: [grapics]
fullview: false
comments: false
---
#### problem description
Use WebGL to implement an online program called BuildingBlocks that allows a user
to build something meaningful from the following six building blocks.
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
In the code above, I split the create block into two part. The first is to generate
a disk. the second one is to generate a square block. Besides the block generate
I need to have different functions inside this CPiece function. For example:
```javascript
this.UpdateOffset = function(dx, dy) {
    this.OffsetX += dx;
    this.OffsetY += dy;
}

this.SetOffset = function(dx, dy) {
    this.OffsetX = dx;
    this.OffsetY = dy;
}
```

##### 2. select blocks
After the basic function of creating blocks and set the changeable variable. I
need to have a function that can tell if my cursor is in the block when I click it.
For example:

 ```javascript
this.isInside = function(x, y) {
     var p=this.transform(x, y);
     for (var i=0; i<this.NumVertices-1; i++) {
     if (this.isInTriangle(p[0], p[1],this.points[0][0],this.points[0][1], this.points[i][0],this.points[i][1],this.points[i+1][0],this.points[i+1][1])) return true;
     }
     return false;
}
```
As you see, I use isInTriangle() to tell if the cursor is in. The basic idea is that
because my circle or square is based on triangle fan, I can divide the shape in to
multiple triangle. After that, I can loop through different triangles to tell if
it is in the block like this:
```javascript
this.isInTriangle = function(px,py,ax,ay,bx,by,cx,cy){ //check if in triangle
   var v0 = [cx-ax,cy-ay];
   var v1 = [bx-ax,by-ay];
   var v2 = [px-ax,py-ay];

   var dot00 = (v0[0]*v0[0]) + (v0[1]*v0[1]);
   var dot01 = (v0[0]*v1[0]) + (v0[1]*v1[1]);
   var dot02 = (v0[0]*v2[0]) + (v0[1]*v2[1]);
   var dot11 = (v1[0]*v1[0]) + (v1[1]*v1[1]);
   var dot12 = (v1[0]*v2[0]) + (v1[1]*v2[1]);

   var invDenom = 1/ (dot00 * dot11 - dot01 * dot01);
   var u = (dot11 * dot02 - dot01 * dot12) * invDenom;
   var v = (dot00 * dot12 - dot01 * dot02) * invDenom;

   return ((u >= 0) && (v >= 0) && (u + v < 1));
  }
```
##### 3. re-render the canvas
This part is easier. Call the function after push the blocks.
```javascript
window.requestAnimFrame(render);
```
##### 4. delete blocks
Delete blocks is just check if the cursor in the shape and pop it out from the array.
```Javascript
for (var i=Blocks.length-1; i>=0; i--) {	// search from last to first
      if (Blocks[i].isInside(x, y)) {
        // move Blocks[i] to the top
        var temp=Blocks[i];
        for (var j=i; j<Blocks.length-1; j++) Blocks[j]=Blocks[j+1];
        Blocks[Blocks.length-1]=temp;
        // pop out the blocks
        Blocks.pop();
        // redraw
        // render();
        window.requestAnimFrame(render);
        return;
}
```
The above is sometime I want to adress on this project.


[Project Link](https://scao7.github.io/cs435/project2/Buildingblocks.html)
