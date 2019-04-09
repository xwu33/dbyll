---
layout: post
title: Computer Graphics ---- Texture
categories: [Graphics]
tags: [Graphics,JavaScript]
fullview: false
comments: false
---
#### Problem Description
We want to draw three walls and put texture on them.At the same time we want to create a TV that we can change the frame on the screen every 1 second. You are able to turn on and off the TV.


#### In the vertex and fragment shader
In the vertex shader we didn't change anything, we only need to add texture variable to the fragment shader.

##### <span style="color:blue"> fragment shader </span>
```javascript
<script id="fragment-shader" type="x-shader/x-fragment">
precision mediump float;
varying vec4 fColor;
varying  vec2 fTexCoord; // a texture coordinate
uniform sampler2D texture; //2D images

void
main()
{
    gl_FragColor = fColor * texture2D( texture, fTexCoord );
}
</script>
```
In the HTML file we also need to include the image that we used. I use 512 * 512 pixel images.
We need to link the image to configure and texture
##### <span style="color:blue"> include image</span>
```html
<img id = "texImage" src = "SA2011_black.gif" hidden></img>
<img id = "texImage2" src = "a1.gif" hidden></img>
<img id = "texImage1" src = "a2.gif" hidden></img>
<img id = "texImage3" src = "black.gif" hidden></img>
<img id = "texImage4" src = "wood.gif" hidden></img>
<img id = "texImage5" src = "wallPaper1.gif" hidden></img>
<img id = "texImage6" src = "floor.gif" hidden></img>
<img id = "texImage7" src = "metal.gif" hidden></img>
```

#### In JavaScript file
In the JavaScript file we need to configure each texture first here is the configure function
##### <span style="color:blue"> texture configure function </span>

```javascript
function configureTexture(image) {
  texture = gl.createTexture();
  gl.bindTexture(gl.TEXTURE_2D, texture);
  gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL, true);
  gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB,
  gl.RGB, gl.UNSIGNED_BYTE, image);
  gl.generateMipmap(gl.TEXTURE_2D);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER,
  gl.NEAREST_MIPMAP_LINEAR);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.NEAREST);

 // gl.uniform1i(gl.getUniformLocation(program, "texture"), 0);

  return texture;
}
```
I returned a texture here

#### Texture Binding
The last step is binding the texture and and load to the program
##### <span style="color:blue"> passing value to fragment shader </span>
```javascript
var vTexCoord = gl.getAttribLocation(program, "vTexCoord");
gl.vertexAttribPointer(vTexCoord, 2, gl.FLOAT, false, 0, 0);
gl.enableVertexAttribArray(vTexCoord);
```
##### <span style="color:blue"> get image and configure</span>
```javascript
image[0] = document.getElementById("texImage");
image[1] = document.getElementById("texImage1");
image[2] = document.getElementById("texImage2");
image[3] = document.getElementById("texImage3");
image[4] = document.getElementById("texImage4");
image[5] = document.getElementById("texImage5");
image[6] = document.getElementById("texImage6");
image[7] = document.getElementById("texImage7");
wallPaper = configureTexture(image[5]);
holderPaper = configureTexture(image[4]);
TVPaper = configureTexture(image[0]);
floorPaper = configureTexture(image[6]);
metalPaper = configureTexture(image[7]);
configureTexture(image[change]);
```
##### <span style="color:blue"> binding before draws the geometer </span>
Binding for each object and draw,
```javascript
function drawTV() {
  var s = scalem(0.8, 0.55, 0.8);
  var instanceMatrix = mult(translate(0.0, 0.32, 0.1), s);
  var t = mult(modelViewMatrix, instanceMatrix);
  gl.uniformMatrix4fv(modelViewMatrixLoc, false, flatten(t));
  gl.bindTexture(gl.TEXTURE_2D,TVPaper); //binding before drawTV
  gl.drawArrays(gl.TRIANGLES, 24, 6);
}
```
##### <span style="color:blue"> rendering</span>
```javascript
var render = function () {
  gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
  drawTV();
  leftWall();
  rightWall();
  backWall();
  holder();
  backBoard();
  floor();
  table();
  tableLeg1();
  tableLeg2();
  neck();
  requestAnimFrame(render);
}
```

[Project Link](https://scao7.github.io/cs435/project5/texmap.html)
[resume link](https://shengtingcao.top/assets/media/resumeShengtingCao.pdf")
