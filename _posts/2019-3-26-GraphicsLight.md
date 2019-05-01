---
layout: post
title: Computer Graphics ---- Lighting
categories: [Graphics]
tags: [Graphics,JavaScript]
fullview: false
comments: false
shortinfo: There are two different way to mimic a light on the material. One is vertex shader the other is fragment shader. In this project we need to create a spotlight aiming on a dark square stage. We should be able to switch the different shading method and also moving the light.
---
#### Problem Description
There are two different way to mimic a light on the material. One is vertex shader the other is fragment shader. In this project we need to create a spotlight aiming on a dark square stage. We should be able to switch the different shading method and also moving the light.

Simply saying that, switch from fragment shader to vertex shader just simply do all the calculation in the vertex shader instead of fragment shader by using varying variables.
#### vertex shader based
##### <span style="color:blue"> vertex shader 顶点着色器为主 </span>
```javascript
<script id="vertex-shader" type="x-shader/x-vertex">
  attribute vec4 a_position;
  attribute vec3 a_normal;

  uniform vec3 u_lightWorldPosition;
  uniform vec3 u_viewWorldPosition;
  uniform mat4 u_world;
  uniform mat4 u_worldViewProjection;
  uniform mat4 u_worldInverseTranspose;
  varying vec3 v_normal;
  varying vec3 v_surfaceToLight;
  varying vec3 v_surfaceToView;
  varying vec4 fColor;
  uniform vec4 u_color;
  uniform float u_shininess;
  uniform vec3 u_lightDirection;
  uniform float u_innerLimit;          // in dot space
  uniform float u_outerLimit;          // in dot space
  varying float light;
  varying float specular;
  void main() {
    // Multiply the position by the matrix.
    gl_Position = u_worldViewProjection * a_position;
    // orient the normals and pass to the fragment shader
    v_normal = mat3(u_worldInverseTranspose) * a_normal;
    // compute the world position of the surfoace
    vec3 surfaceWorldPosition = (u_world * a_position).xyz;
    // compute the vector of the surface to the light
    // and pass it to the fragment shader
    v_surfaceToLight = u_lightWorldPosition - surfaceWorldPosition;
    // compute the vector of the surface to the view/camera
    // and pass it to the fragment shader
    v_surfaceToView = u_viewWorldPosition - surfaceWorldPosition;
    // because v_normal is a varying it's interpolated
    // we it will not be a uint vector. Normalizing it
    // will make it a unit vector again
    vec3 normal = normalize(v_normal);
    vec3 surfaceToLightDirection = normalize(v_surfaceToLight);
    vec3 surfaceToViewDirection = normalize(v_surfaceToView);
    vec3 halfVector = normalize(surfaceToLightDirection + surfaceToViewDirection);
    float dotFromDirection = dot(surfaceToLightDirection,
                                 -u_lightDirection);
    float inLight = smoothstep(u_outerLimit, u_innerLimit, dotFromDirection);
    light = inLight * dot(normal, surfaceToLightDirection);
    specular = inLight * pow(dot(normal, halfVector), u_shininess);
    fColor = u_color;
  }
</script>
<!-- fragment shader program -->
<script id="fragment-shader" type="x-shader/x-fragment">
  precision mediump float;
  varying vec4 fColor;
  // Passed in from the vertex shader.
  varying vec3 v_normal;
  varying vec3 v_surfaceToLight;
  varying vec3 v_surfaceToView;
  varying float light;
  varying float specular;
  void main() {
    gl_FragColor = fColor;
    // Lets multiply just the color portion (not the alpha)
    // by the light
    gl_FragColor.rgb *= light;
    // Just add in the specular
    gl_FragColor.rgb += specular;
  }
</script>

```

#### fragment shader based
##### <span style="color:blue"> fragment shader 片段着色器为主 </span>
```javascript
<script id="vertex-shader" type="x-shader/x-vertex">
attribute vec4 a_position;
attribute vec3 a_normal;
uniform vec4 ambientProduct, diffuseProduct, specularProduct;
uniform vec3 u_lightWorldPosition;
uniform vec3 u_viewWorldPosition;
uniform mat4 u_world;
uniform mat4 u_worldViewProjection;
uniform mat4 u_worldInverseTranspose;
varying vec3 v_normal;
varying vec3 v_surfaceToLight;
varying vec3 v_surfaceToView;
void main() {
  // Multiply the position by the matrix.
  gl_Position = u_worldViewProjection * a_position;
  // orient the normals and pass to the fragment shader
  v_normal = mat3(u_worldInverseTranspose) * a_normal;
  // compute the world position of the surfoace
  vec3 surfaceWorldPosition = (u_world * a_position).xyz;
  // compute the vector of the surface to the light
  // and pass it to the fragment shader
  v_surfaceToLight = u_lightWorldPosition - surfaceWorldPosition;
  // compute the vector of the surface to the view/camera
  // and pass it to the fragment shader
  v_surfaceToView = u_viewWorldPosition - surfaceWorldPosition;
}
</script>


```
[Reference Page]([Project Link](https://scao7.github.io/cs435/project4/spotlight.html))
[Project Link](https://scao7.github.io/cs435/project4/spotlight.html)
