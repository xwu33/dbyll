---
layout: post
title: Graphics Mid Term review concepts
categories: [Graphics, exams]
tags: [Graphics]
fullview: false
comments: false
---
#### concepts
In addition to local local varibale in shader, there are tree unique type of varialbes
##### <span style="color:blue">three unique varialbes</span>
- attribute variables
- uniform variables
- varying varialbes

##### <span style="color:blue">OpenGL graphics pipeline</span>
form vertices to pixels
- vertex processor
- clipper and primitive assembler
- rasterizer
- fragment processor

##### <span style="color:blue">shading language</span>

- vertex shader
- fragment shader

##### <span style="color:blue">webGL Organization</span>

![image info](assets/media/graphics/Picture1.jpg)

#### Instance Transformation

```JavaScript
  translate( x, y, z ); //example: translate(-1,0,0);
  rotate(angle, axis); //examples: rotate(90,0,1,0);
  scalem(x,y,z);// scalem(1,1,1);
```

#### Model View

```JavaScript
lookAt(eye, at, up);
ortho(left, right ,bottom top, near, far);

```

[reference](https://www.cs.unm.edu/~angel/WebGL/7E/  )
