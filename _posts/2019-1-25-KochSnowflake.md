---
layout: post
title: webGL for Koch snowflake
categories: [Graphics]
tags: [Graphics,JavaScript]
fullview: false
comments: false
shortinfo: Draw the Koch snowflake using recursive method!
---
#### Problem Description
Draw the Koch snowflake using recursive method!

#### calculate the point for the Koch lines
    function calculatePoint(center, p){
      var angleInDegrees = 60;
      var angleInRadians = angleInDegrees * Math.PI / 180;
      var s1 = Math.sin(angleInRadians);
      var c1 = Math.cos(angleInRadians);
      var x1 = (p[0] - center[0]) * c1 - (p[1] - center[1])* s1 + center[0];
      var y1 = (p[0] - center[0]) * s1 + (p[1] - center[1])* c1 + center[1];
      var f = vec2(x1,y1);
      return f;
    }

#### divide line into koch line and call the function recursively
    function divideLine(a, b,count)
    {
      if(count === 0){
        var left;
        var right;
        left = mix(a,b,1/3);
        right = mix(a,b,2/3);
        var f = calculatePoint(left,right);
        drawLine(a,left,f,right,b);
      }
      else {
        var ab = mix (a,b,0.3333);
        var ba = mix (b,a,0.3333);
        var v = calculatePoint(ab, ba);
        count --;
        divideLine(a,ab,count);
        divideLine(ba,b,count);
        divideLine(ab,v,count);
        divideLine(v,ba,count);
      }
        return f;
    }

#### to draw a snowfalke by calling the divideLine function three times
    function snowflake(a,b,c,count){
        divideLine(a,b,count);
        divideLine(b,c,count);
        divideLine(c,a,count);
    }

[Project Link](https://scao7.github.io/cs435/project1/snowflake.html)

more coming!
